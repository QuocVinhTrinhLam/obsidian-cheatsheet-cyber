## Discovery/Footprinting

The Splunk web server runs by default on port 8000. On older versions of Splunk, the default credentials are `admin:changeme`, which are conveniently displayed on the login page.
![](Pasted%20image%2020260730125303.png)

The latest version of Splunk sets credentials during the installation process. If the default credentials do not work, it is worth checking for common weak passwords such as `admin`, `Welcome`, `Welcome1`, `Password123`, etc.
![](Pasted%20image%2020260730125318.png)

```shell
3kjS@htb[/htb]$ sudo nmap -sV 10.129.201.50
```
![](Screenshot%202026-07-30%20at%2012.53.38.png)
## Enumeration

```url
https://10.129.201.50:8000/en-US/app/launcher/home
```
![](Pasted%20image%2020260730125406.png)
