# [Situational Awareness](Situational%20Awareness.md)
### What is the IP address of the other NIC attached to the target host?

```cmd
ipconfig /all
```
![](Screenshot%202026-08-14%20at%2015.02.25.png)
### What executable other than cmd.exe is blocked by AppLocker?

```powershell
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```
![](Screenshot%202026-08-14%20at%2015.06.37.png)