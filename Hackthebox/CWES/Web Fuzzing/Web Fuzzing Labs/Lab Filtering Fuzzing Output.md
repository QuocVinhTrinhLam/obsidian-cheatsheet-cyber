# [Filtering Fuzzing Output](Filtering%20Fuzzing%20Output.md)
### What flag do you find when successfully fuzzing the POST parameter?

```sh
ffuf -u http://154.57.164.72:31394/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -v -mc 200
```
![](Screenshot%202026-09-09%20at%2013.12.46.png)

```sh
curl -d "y=SUNWmc" http://154.57.164.72:31394/post.php

HTB{p0st_fuzz1ng_succ3ss}
```
