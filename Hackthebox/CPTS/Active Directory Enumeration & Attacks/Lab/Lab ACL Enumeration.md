# [ACL Enumeration](ACL%20Enumeration.md)

## What is the rights GUID for User-Force-Change-Password?

```powershell
$sid = Convert-NametoSid wley

Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid}
```
## What flag can we use with PowerView to show us the ObjectAceType in a human-readable format during our enumeration?

`ResolveGUIDs`
## What privileges does the user damundsen have over the Help Desk Level 1 group?

```powershell
$sid2 = Convert-NameToSid damundsen

Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid2} -Verbose
```
![](Pasted%20image%2020260528164048.png)
## Using the skills learned in this section, enumerate the ActiveDirectoryRights that the user forend has over the user dpayne (Dagmar Payne).
```powershell
$sid2 = Convert-NameToSid forend

Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid2}
```
![](Screenshot%202026-05-28%20at%2016.46.05.png)
## What is the ObjectAceType of the first right that the forend user has over the GPO Management group? (two words in the format Word-Word)

```powershell
.\SharpHound.exe -c All --zipfilename ILFREIGHT
```
![](Screenshot%202026-05-28%20at%2017.10.56.png)
