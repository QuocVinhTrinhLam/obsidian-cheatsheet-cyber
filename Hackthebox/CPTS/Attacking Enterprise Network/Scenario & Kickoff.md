# External Information Gathering

```shell
3kjS@htb[/htb]$ sudo nmap --open -oA inlanefreight_ept_tcp_1k -iL scope

Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-20 14:56 EDT
Nmap scan report for 10.129.203.101
Host is up (0.12s latency).
Not shown: 989 closed tcp ports (reset)
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
25/tcp   open  smtp
53/tcp   open  domain
80/tcp   open  http
110/tcp  open  pop3
111/tcp  open  rpcbind
143/tcp  open  imap
993/tcp  open  imaps
995/tcp  open  pop3s
8080/tcp open  http-proxy

Nmap done: 1 IP address (1 host up) scanned in 2.25 seconds
```

```shell
3kjS@htb[/htb]$ sudo nmap --open -p- -A -oA inlanefreight_ept_tcp_all_svc -iL scope
```

We can use this handy Nmap grep [cheatsheet](https://github.com/leonjza/awesome-nmap-grep) to "cut through the noise" and extract the most useful information from the scan.
```shell
3kjS@htb[/htb]$ egrep -v "^#|Status: Up" inlanefreight_ept_tcp_all_svc.gnmap | cut -d ' ' -f4- | tr ',' '\n' | \                                                               
sed -e 's/^[ \t]*//' | awk -F '/' '{print $7}' | grep -v "^$" | sort | uniq -c \
| sort -k 1 -nr

      2 Dovecot pop3d
      2 Dovecot imapd (Ubuntu)
      2 Apache httpd 2.4.41 ((Ubuntu))
      1 vsftpd 3.0.3
      1 Postfix smtpd
      1 OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
      1 2-4 (RPC #100000)
```

We know from the scoping sheet that the primary domain is `INLANEFREIGHT.LOCAL`, so let's see what we can find.
```shell
3kjS@htb[/htb]$ dig axfr inlanefreight.local @10.129.203.101
```
![](Screenshot%202026-08-30%20at%2014.56.53.png)

```shell
3kjS@htb[/htb]$ curl -s -I http://10.129.203.101 -H "HOST: defnotvalid.inlanefreight.local" | grep "Content-Length:"

Content-Length: 15157
```

```shell
3kjS@htb[/htb]$ ffuf -w namelist.txt:FUZZ -u http://10.129.203.101/ -H 'Host:FUZZ.inlanefreight.local' -fs 15157
```
![](Screenshot%202026-08-30%20at%2014.57.46.png)
## Enumeration Results

```shell
3kjS@htb[/htb]$ sudo tee -a /etc/hosts > /dev/null <<EOT

## inlanefreight hosts 
10.129.203.101 inlanefreight.local blog.inlanefreight.local careers.inlanefreight.local dev.inlanefreight.local gitlab.inlanefreight.local ir.inlanefreight.local status.inlanefreight.local support.inlanefreight.local tracking.inlanefreight.local vpn.inlanefreight.local
EOT
```
