## Domain Trusts Overview
![](Pasted%20image%2020260601130632.png)
#### Trust Table Side By Side
![](Pasted%20image%2020260601130638.png)
## Enumerating Trust Relationships
#### Using Get-ADTrust

```powershell
PS C:\htb> Import-Module activedirectory 
PS C:\htb> Get-ADTrust -Filter *
```
#### Checking for Existing Trusts using Get-DomainTrust

```powershell
PS C:\htb> Get-DomainTrust
```
#### Using Get-DomainTrustMapping

```powershell
PS C:\htb> Get-DomainTrustMapping
```
#### Checking Users in the Child Domain using Get-DomainUser

```powershell
PS C:\htb> Get-DomainUser -Domain LOGISTICS.INLANEFREIGHT.LOCAL | select SamAccountName
```
#### Using netdom to query domain trust

```powershell
C:\htb> netdom query /domain:inlanefreight.local trust
```
#### Using netdom to query domain controllers

```powershell
C:\htb> netdom query /domain:inlanefreight.local dc
```
#### Using netdom to query workstations and servers

```powershell
C:\htb> netdom query /domain:inlanefreight.local workstation
```
#### Visualizing Trust Relationships in BloodHound
![](Pasted%20image%2020260601131043.png)