# [DCSync](DCSync.md)
### Perform a DCSync attack and look for another user with the option "Store password using reversible encryption" set. Submit the username as your answer.

```powershell
Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl
```
![](Screenshot%202026-05-29%20at%2015.34.54.png)
### What is this user's cleartext password?
