# [Parameter and Value Fuzzing](Parameter%20and%20Value%20Fuzzing.md)
### What flag do you find when successfully fuzzing the GET parameter?

```sh
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -fc 404 -u "http://154.57.164.67:30902/get.php?x=FUZZ"
```
![](Screenshot%202026-09-09%20at%2012.24.36.png)

```sh
curl http://154.57.164.67:30902/get.php?x=OA_HTML
```
![](Screenshot%202026-09-09%20at%2012.24.54.png)
### What flag do you find when successfully fuzzing the POST parameter?
```sh
ffuf -u http://154.57.164.67:30902/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200 -v
```
![](Screenshot%202026-09-09%20at%2012.20.54.png)
![](Screenshot%202026-09-09%20at%2012.21.23.png)

```sh
curl -d 'y=SUNWmc' http://154.57.164.67:30902/post.php
```
![](Screenshot%202026-09-09%20at%2012.21.43.png)