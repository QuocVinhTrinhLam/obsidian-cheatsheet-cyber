# [Special Permissions](Special%20Permissions.md)
### Find a file with the setuid bit set that was not shown in the section command output (full path to the binary).

```shell
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null
```
![](Screenshot%202026-08-05%20at%2013.45.36.png)
### Find a file with the setgid bit set that was not shown in the section command output (full path to the binary).

```shell
find / -user root -perm -2000 -exec ls -ldb {} \; 2>/dev/null
```
![](Screenshot%202026-08-05%20at%2013.52.13.png)
