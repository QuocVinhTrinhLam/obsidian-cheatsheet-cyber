# [Domain Reconnaissance](Domain%20Reconnaissance.md)

![](Screenshot%202026-11-09%20at%2015.53.50.png)
### Submit the relative identifier of the SID for user websec

```powershell
Get-DomainUser -Identity websec | Select-Object samaccountname, objectsid
```

![](Screenshot%202026-11-09%20at%2015.58.07.png)
### Enumerate the domain and submit its domain mode level

```powershell
(Get-Domain).DomainModeLevel
```

![](Screenshot%202026-11-09%20at%2016.07.44.png)
### Use any of the techniques to obtain the maximum password age

```powershell
Get-DomainPolicyData | Select-Object -ExpandProperty SystemAccess
```

![](Screenshot%202026-11-09%20at%2016.01.29.png)
### Submit the DNS forest name

```powershell
Get-NetForest | Select-Object Name
```

![](Screenshot%202026-11-09%20at%2016.04.27.png)
### Submit the external domain admin (username)

```powershell
Get-DomainForeignUser
```

![](Screenshot%202026-11-09%20at%2016.14.18.png)
### Submit the internal IP address of WEB01

```powershell
ipconfig
```

![](Screenshot%202026-11-09%20at%2016.08.15.png)