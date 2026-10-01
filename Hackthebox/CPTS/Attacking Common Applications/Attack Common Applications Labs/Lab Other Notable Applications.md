# [Other Notable Applications](Other%20Notable%20Applications.md)
### Enumerate the target host and identify the running application. What application is running?

```shell
sudo nmap 10.129.201.102 -sC -sV
```
![](Screenshot%202026-08-03%20at%2012.48.02.png)
### Enumerate the application for vulnerabilities. Gain remote code execution and submit the contents of the flag.txt file on the administrator desktop.

```shell
gobuster dir -u http://10.129.201.102:7001/ -w /usr/share/wordlists/dirb/small.txt
```
![](Screenshot%202026-08-03%20at%2013.00.12.png)
![](Screenshot%202026-08-03%20at%2013.00.28.png)

I scrolled down and saw the version of the server `12.2.1.3.0`
![](Screenshot%202026-08-03%20at%2013.01.09.png)

I moved to Metasploit to exploit, using exploit(multi/http/weblogic_admin_handle_rce)
![](Screenshot%202026-08-03%20at%2013.04.22.png)

Successfully!!
![](Screenshot%202026-08-03%20at%2013.07.13.png)
![](Screenshot%202026-08-03%20at%2013.09.47.png)