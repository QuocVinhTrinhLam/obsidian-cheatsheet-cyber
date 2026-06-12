## Extension Fuzzing

```shell
3kjS@htb[/htb]$ ffuf -w /opt/useful/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ <SNIP>
```

```shell
3kjS@htb[/htb]$ ffuf -w /opt/useful/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://SERVER_IP:PORT/blog/indexFUZZ
```
![](Screenshot%202026-06-10%20at%2020.46.05.png)
## Page Fuzzing

```shell
3kjS@htb[/htb]$ ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php
```
![](Screenshot%202026-06-10%20at%2020.46.41.png)
