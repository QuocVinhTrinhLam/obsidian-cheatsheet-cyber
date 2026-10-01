## Key Data Points

`OS name`: Knowing the type of Windows OS (workstation or server) and level (Windows 7 or 10, Server 2008, 2012, 2016, 2019, etc.) will give us an idea of the types of tools that may be available (such as the `PowerShell` version), or lack thereof on legacy systems. This would also identify the operating system version for which there may be public exploits available.

`Version`: As with the OS [version](https://en.wikipedia.org/wiki/Comparison_of_Microsoft_Windows_versions), there may be public exploits that target a vulnerability in a specific version of Windows. Windows system exploits can cause system instability or even a complete crash. Be careful running these against any production system, and make sure you fully understand the exploit and possible ramifications before running one.

`Running Services`: Knowing what services are running on the host is important, especially those running as `NT AUTHORITY\SYSTEM` or an administrator-level account. A misconfigured or vulnerable service running in the context of a privileged account can be an easy win for privilege escalation.
## System Information
#### Tasklist

```cmd
C:\htb> tasklist /svc
```
#### Display All Environment Variables

```cmd
C:\htb> set
```
#### View Detailed Configuration Information

```cmd
C:\htb> systeminfo
```
#### Patches and Updates

```cmd
C:\htb> wmic qfe
```

We can do this with PowerShell as well using the [Get-Hotfix](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-hotfix?view=powershell-7.1) cmdlet.

```powershell
PS C:\htb> Get-HotFix | ft -AutoSize
```
#### Installed Programs

```cmd
C:\htb> wmic product get name
```

We can, of course, do this with PowerShell as well using the [Get-WmiObject](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-wmiobject?view=powershell-5.1) cmdlet.

```powershell
PS C:\htb> Get-WmiObject -Class Win32_Product | select Name, Version
```
#### Display Running Processes
#### Netstat

```powershell
PS C:\htb> netstat -ano
```
## User & Group Information
#### Logged-In Users

```cmd
C:\htb> query user
```
#### Current User

```cmd
C:\htb> echo %USERNAME%
```
#### Current User Privileges

```cmd
C:\htb> whoami /priv
```
#### Current User Group Information

```cmd
C:\htb> whoami /groups
```
#### Get All Users

```cmd
C:\htb> net user
```
#### Get All Groups

```cmd
C:\htb> net localgroup
```
#### Details About a Group

```cmd
C:\htb> net localgroup administrators
```
#### Get Password Policy & Other Account Information

```cmd
C:\htb> net accounts
```
