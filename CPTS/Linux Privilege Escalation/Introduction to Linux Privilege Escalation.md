## Enumeration
#### List Current Processes

```shell
3kjS@htb[/htb]$ ps aux | grep root
```
![](Screenshot%202026-08-04%20at%2012.13.45.png)
#### List Current Terminal-Attached Processes

```shell
3kjS@htb[/htb]$ ps au
```
![](Screenshot%202026-08-04%20at%2012.14.09.png)
#### Home Directory Contents

```shell
3kjS@htb[/htb]$ ls /home 

backupsvc bob.jones cliff.moore logger mrb3n shared stacey.jenkins
```
#### User's Home Directory Contents

```shell
3kjS@htb[/htb]$ ls -la /home/stacey.jenkins/
```
![](Screenshot%202026-08-04%20at%2012.14.38.png)
#### SSH Directory Contents

```shell
3kjS@htb[/htb]$ ls -l ~/.ssh 

total 8 
-rw------- 1 mrb3n mrb3n 1679 Aug 30 23:37 id_rsa 
-rw-r--r-- 1 mrb3n mrb3n 393 Aug 30 23:37 id_rsa.pub
```
#### Bash History

```shell
3kjS@htb[/htb]$ history 

		1 id 
		2 cd /home/cliff.moore 
		3 exit 
		4 touch backup.sh 
		5 tail /var/log/apache2/error.log 
		6 ssh ec2-user@dmz02.inlanefreight.local 
		7 history
```
#### Sudo - List User's Privileges

```shell
3kjS@htb[/htb]$ sudo -l 

Matching Defaults entries for sysadm on NIX02: 
	env_reset, mail_badpass, 
secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin 

User sysadm may run the following commands on NIX02: 
	(root) NOPASSWD: /usr/sbin/tcpdump
```
#### Passwd

```shell
3kjS@htb[/htb]$ cat /etc/passwd
```
![](Screenshot%202026-08-04%20at%2012.16.23.png)
#### Cron Jobs

```shell
3kjS@htb[/htb]$ ls -la /etc/cron.daily/
```
![](Screenshot%202026-08-04%20at%2012.16.39.png)
#### File Systems & Additional Drives

```shell
3kjS@htb[/htb]$ lsblk 
```
![](Screenshot%202026-08-04%20at%2012.17.27.png)
#### Find Writable Directories

```shell
3kjS@htb[/htb]$ find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null
```
![](Screenshot%202026-08-04%20at%2012.17.48.png)
#### Find Writable Files

```shell
3kjS@htb[/htb]$ find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null
```
![](Screenshot%202026-08-04%20at%2012.18.04.png)
