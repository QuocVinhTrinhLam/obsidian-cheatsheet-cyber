# [Domain Trusts Primer](Domain%20Trusts%20Primer.md)

### What is the child domain of INLANEFREIGHT.LOCAL? (format: FQDN, i.e., DEV.ACME.LOCAL)

```powershell
Import-Module ActiveDirectory
Get-ADTrust -Filter *
```
![](Screenshot%202026-06-01%20at%2013.21.30.png)
### What domain does the INLANEFREIGHT.LOCAL domain have a forest transitive trust with?
![](Screenshot%202026-06-01%20at%2013.23.33.png)
### What direction is this trust?

`BiDirectional`
