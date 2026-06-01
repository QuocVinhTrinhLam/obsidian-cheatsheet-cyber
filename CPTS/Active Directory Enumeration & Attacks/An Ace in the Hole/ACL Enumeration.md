## Enumerating ACLs with PowerView
#### Using Find-InterestingDomainAcl

```powershell
PS C:\htb> Find-InterestingDomainAcl
```
![](Screenshot%202026-05-28%20at%2015.56.29.png)

```powershell
PS C:\htb> Import-Module .\PowerView.ps1 
PS C:\htb> $sid = Convert-NameToSid wley
```
#### Using Get-DomainObjectACL

```powershell
PS C:\htb> Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $sid}
```
#### Performing a Reverse Search & Mapping to a GUID Value

```powershell
PS C:\htb> $guid= "00299570-246d-11d0-a768-00aa006e0529" 
PS C:\htb> Get-ADObject -SearchBase "CN=Extended-Rights,$((Get-ADRootDSE).ConfigurationNamingContext)" -Filter {ObjectClass -like 'ControlAccessRight'} -Properties * |Select Name,DisplayName,DistinguishedName,rightsGuid| ?{$_.rightsGuid -eq $guid} | fl
```
![](Screenshot%202026-05-28%20at%2016.00.07.png)
#### Using the -ResolveGUIDs Flag

```powershell
PS C:\htb> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid}
```
#### Creating a List of Domain Users

```powershell
PS C:\htb> Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt
```
#### A Useful foreach Loop

```powershell
PS C:\htb> foreach($line in [System.IO.File]::ReadLines("C:\Users\htb-student\Desktop\ad_users.txt")) {get-acl "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match 'INLANEFREIGHT\\wley'}}
```
![](Screenshot%202026-05-28%20at%2016.03.48.png)
#### Further Enumeration of Rights Using damundsen

```powershell
PS C:\htb> $sid2 = Convert-NameToSid damundsen 
PS C:\htb> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid2} -Verbose
```
![](Screenshot%202026-05-28%20at%2016.04.44.png)
#### Investigating the Help Desk Level 1 Group with Get-DomainGroup

```powershell
PS C:\htb> Get-DomainGroup -Identity "Help Desk Level 1" | select memberof

memberof
--------
CN=Information Technology,OU=Security Groups,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL
```
#### Investigating the Information Technology Group

```powershell
PS C:\htb> $itgroupsid = Convert-NameToSid "Information Technology" 
PS C:\htb> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $itgroupsid} -Verbose
```
![](Screenshot%202026-05-28%20at%2016.06.50.png)
#### Looking for Interesting Access

```powershell
PS C:\htb> $adunnsid = Convert-NameToSid adunn 
PS C:\htb> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $adunnsid} -Verbose
```
## Enumerating ACLs with BloodHound
#### Viewing Node Info through BloodHound
![](Pasted%20image%2020260528161300.png)
#### Investigating ForceChangePassword Further
![](Pasted%20image%2020260528161321.png)
#### Viewing Potential Attack Paths through BloodHound
![](Pasted%20image%2020260528161421.png)
#### Viewing Pre-Build queries through BloodHound
![](Pasted%20image%2020260528161444.png)
