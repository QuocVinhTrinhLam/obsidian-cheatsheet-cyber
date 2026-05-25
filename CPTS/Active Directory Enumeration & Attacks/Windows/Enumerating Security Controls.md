## Windows Defender
#### Checking the Status of Defender with Get-MpComputerStatus

```powershell
PS C:\htb> Get-MpComputerStatus
```
## AppLocker

[PowerShell executable locations](https://www.powershelladmin.com/wiki/PowerShell_Executables_File_System_Locations) such as `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe` or `PowerShell_ISE.exe`. We can see that this is the case in the `AppLocker` rules shown below. All Domain Users are disallowed from running the 64-bit PowerShell executable located at:

`%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe`
#### Using Get-AppLockerPolicy cmdlet

```powershell
PS C:\htb> Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```
## PowerShell Constrained Language Mode
#### Enumerating Language Mode

```powershell
PS C:\htb> $ExecutionContext.SessionState.LanguageMode 

ConstrainedLanguage
```
## LAPS
#### Using Find-LAPSDelegatedGroups

```powershell
PS C:\htb> Find-LAPSDelegatedGroups
```
#### Using Find-AdmPwdExtendedRights

```powershell
PS C:\htb> Find-AdmPwdExtendedRights
```
#### Using Get-LAPSComputers

```powershell
PS C:\htb> Get-LAPSComputers
```
