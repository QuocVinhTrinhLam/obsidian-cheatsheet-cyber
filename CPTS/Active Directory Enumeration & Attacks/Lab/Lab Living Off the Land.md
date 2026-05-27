# [Living Off the Land](CPTS/Active%20Directory%20Enumeration%20&%20Attacks/Living%20Off%20the%20Land.md)

## Q1) Enumerate the host's security configuration information and provide its AMProductVersion.
`Get-MpComputerStatus`
![](Screenshot%202026-05-27%20at%2017.50.12.png)
## Q2) What domain user is explicitly listed as a member of the local Administrators group on the target host?
`net localgroup administrators`
![](Screenshot%202026-05-27%20at%2017.51.13.png)
## Q3) Utilizing techniques learned in this section, find the flag hidden in the description field of a disabled account with administrative privileges. Submit the flag as the answer.
```powershell
dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2))" -attr distinguishedName userAccountControl,
description
```
![](Screenshot%202026-05-27%20at%2017.51.59.png)