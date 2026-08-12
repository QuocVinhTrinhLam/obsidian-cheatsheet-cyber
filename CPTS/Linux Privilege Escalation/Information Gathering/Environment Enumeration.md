Enumeration is the key to privilege escalation. Several helper scripts (such as [LinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS) and [LinEnum](https://github.com/rebootuser/LinEnum)) exist to assist with enumeration.
## Gaining Situational Awareness

Typically we'll want to run a few basic commands to orient ourselves:

- `whoami` - what user are we running as
- `id` - what groups does our user belong to?
- `hostname` - what is the server named, can we gather anything from the naming convention?
- `ifconfig` or `ip a` - what subnet did we land in, does the host have additional NICs in other subnets?
- `sudo -l` - can our user run anything with sudo (as another user as root) without needing a password? This can sometimes be the easiest win and we can do something like `sudo su` and drop right into a root shell.

We'll start out by checking out what operating system and version we are dealing with.

```shell
3kjS@htb[/htb]$ cat /etc/os-release
```
![](Screenshot%202026-08-04%20at%2012.20.49.png)

```shell
3kjS@htb[/htb]$ echo $PATH 

/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

We can also check out all environment variables that are set for our current user, we may get lucky and find something sensitive in there such as a password. We'll note this down and move on.

```shell
3kjS@htb[/htb]$ env 

SHELL=/bin/bash 
PWD=/home/htb-student 
LOGNAME=htb-student 
XDG_SESSION_TYPE=tty 
MOTD_SHOWN=pam 
HOME=/home/htb-student 
LANG=en_US.UTF-8 
<SNIP>
```

We can do this a few ways, another way would be `cat /proc/version` but we'll use the `uname -a` command.

```shell
3kjS@htb[/htb]$ uname -a 

Linux nixlpe02 5.4.0-122-generic #138-Ubuntu SMP Wed Jun 22 15:00:31 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```

We can next gather some additional information about the host itself such as the CPU type/version:

```shell
3kjS@htb[/htb]$ lscpu
```
![](Screenshot%202026-08-04%20at%2012.23.35.png)

```shell
3kjS@htb[/htb]$ cat /etc/shells
```
![](Screenshot%202026-08-04%20at%2012.23.55.png)

First, we can use the `lsblk` command to enumerate information about block devices on the system (hard disks, USB drives, optical drives, etc.).

```shell
3kjS@htb[/htb]$ lsblk
```
![](Screenshot%202026-08-04%20at%2012.24.23.png)
#### Existing Users

```shell
3kjS@htb[/htb]$ cat /etc/passwd
```
![](Screenshot%202026-08-04%20at%2012.24.48.png)

```shell
3kjS@htb[/htb]$ cat /etc/passwd | cut -f1 -d:
```
![](Screenshot%202026-08-04%20at%2012.25.08.png)

With Linux, several different hash algorithms can be used to make the passwords unrecognizable. Identifying them from the first hash blocks can help us to use and work with them later if needed. Here is a list of the most used ones:

|**Algorithm**|**Hash**|
|---|---|
|Salted MD5|`$1$`...|
|SHA-256|`$5$`...|
|SHA-512|`$6$`...|
|BCrypt|`$2a$`...|
|Scrypt|`$7$`...|
|Argon2|`$argon2i$`...|

```shell
3kjS@htb[/htb]$ grep "sh$" /etc/passwd
```
![](Screenshot%202026-08-04%20at%2012.28.34.png)
#### Existing Groups

```shell
3kjS@htb[/htb]$ cat /etc/group
```
![](Screenshot%202026-08-04%20at%2012.28.50.png)
#### Mounted File Systems

```shell
3kjS@htb[/htb]$ df -h
```
![](Screenshot%202026-08-04%20at%2012.29.17.png)
#### Unmounted File Systems

```shell
3kjS@htb[/htb]$ cat /etc/fstab | grep -v "#" | column -t
```
![](Screenshot%202026-08-04%20at%2012.29.32.png)
#### All Hidden Files

```shell
3kjS@htb[/htb]$ find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null | grep htb-student
```
![](Screenshot%202026-08-04%20at%2012.29.46.png)
#### All Hidden Directories

```shell
3kjS@htb[/htb]$ find / -type d -name ".*" -ls 2>/dev/null
```
![](Screenshot%202026-08-04%20at%2012.30.04.png)
#### Temporary Files

```shell
3kjS@htb[/htb]$ ls -l /tmp /var/tmp /dev/shm
```
![](Screenshot%202026-08-04%20at%2012.30.22.png)
