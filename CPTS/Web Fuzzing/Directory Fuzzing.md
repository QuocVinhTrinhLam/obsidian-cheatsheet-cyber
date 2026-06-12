## Ffuf

```shell
3kjS@htb[/htb]$ ffuf -h
```
## Directory Fuzzing

```shell
3kjS@htb[/htb]$ ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ
```

```shell
3kjS@htb[/htb]$ ffuf -w <SNIP> -u http://SERVER_IP:PORT/FUZZ
```

```shell
3kjS@htb[/htb]$ ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ
```
![](Screenshot%202026-06-10%20at%2020.36.08.png)