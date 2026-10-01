## Internals
#### Network Interfaces

```shell
3kjS@htb[/htb]$ ip a
```
![](Screenshot%202026-08-04%20at%2012.51.12.png)
#### Hosts

```shell
3kjS@htb[/htb]$ cat /etc/hosts
```
![](Screenshot%202026-08-04%20at%2012.51.27.png)
#### User's Last Login

```shell
3kjS@htb[/htb]$ lastlog
```
![](Screenshot%202026-08-04%20at%2012.51.44.png)
#### Logged In Users

```shell
3kjS@htb[/htb]$ w
```
![](Screenshot%202026-08-04%20at%2012.52.03.png)
#### Command History

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
#### Finding History Files

```shell
3kjS@htb[/htb]$ find / -type f \( -name *_hist -o -name *_history \) -exec ls -l {} \; 2>/dev/null 

-rw------- 1 htb-student htb-student 387 Nov 27 14:02 /home/htb-student/.bash_history
```
#### Cron

```shell
3kjS@htb[/htb]$ ls -la /etc/cron.daily/
```
![](Screenshot%202026-08-04%20at%2012.53.01.png)
#### Proc

```shell
3kjS@htb[/htb]$ find /proc -name cmdline -exec cat {} \; 2>/dev/null | tr " " "\n" 
```
## Services
#### Installed Packages

```shell
3kjS@htb[/htb]$ apt list --installed | tr "/" " " | cut -d" " -f1,3 | sed 's/[0-9]://g' | tee -a installed_pkgs.list
```
#### Sudo Version

```shell
3kjS@htb[/htb]$ sudo -V
```
#### Binaries

```shell
3kjS@htb[/htb]$ ls -l /bin /usr/bin/ /usr/sbin/
```
#### GTFObins

```shell
3kjS@htb[/htb]$ for i in $(curl -s https://gtfobins.org/api.json | jq -r '.executables | keys[]'); do if grep -q "$i" installed_pkgs.list; then echo "Check for GTFO: $i";fi; done
```
#### Trace System Calls

```shell
3kjS@htb[/htb]$ strace ping -c1 10.129.112.20
```
#### Configuration Files

```shell
3kjS@htb[/htb]$ find / -type f \( -name *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null
```
#### Scripts

```shell
3kjS@htb[/htb]$ find / -type f -name "*.sh" 2>/dev/null | grep -v "src\|snap\|share"
```
#### Running Services by User

```shell
3kjS@htb[/htb]$ ps aux | grep root
```
