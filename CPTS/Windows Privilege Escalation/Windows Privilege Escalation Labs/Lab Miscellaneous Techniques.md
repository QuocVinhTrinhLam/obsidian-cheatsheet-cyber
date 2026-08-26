# [Miscellaneous Techniques](CPTS/Windows%20Privilege%20Escalation/Additional%20Techniques/Miscellaneous%20Techniques.md)
### Using the techniques in this section, find the cleartext password for an account on the target host.

```powershell
Get-LocalUser | Select-Object Name, Description
```
![](Screenshot%202026-08-24%20at%2017.05.37.png)
