## Filtering

```shell
3kjS@htb[/htb]$ ffuf -h
```
![](Screenshot%202026-06-11%20at%2021.37.27.png)

```shell
3kjS@htb[/htb]$ ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs 900
```
![](Screenshot%202026-06-11%20at%2021.37.42.png)
