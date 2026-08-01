## Discovery/Footprinting

```url
http://app-dev.inlanefreight.local:8080/invalid
```
![](Pasted%20image%2020260729134131.png)

```shell
3kjS@htb[/htb]$ curl -s http://app-dev.inlanefreight.local:8080/docs/ | grep Tomcat

<html lang="en"><head><META http-equiv="Content-Type" content="text/html; charset=UTF-8"><link href="./images/docs-stylesheet.css" rel="stylesheet" type="text/css"><title>Apache Tomcat 9 (9.0.30) - Documentation Index</title><meta name="author" 

<SNIP>
```

This is the default documentation page, which may not be removed by administrators. Here is the general folder structure of a Tomcat installation.
![](Screenshot%202026-07-29%20at%2013.42.20.png)

Each folder inside `webapps` is expected to have the following structure.
![](Screenshot%202026-07-29%20at%2013.42.42.png)
## Enumeration

```shell
3kjS@htb[/htb]$ gobuster dir -u http://web01.inlanefreight.local:8180/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
```
