## Brute-Force Attack

For instance, consider the following web application that assigns a four-character session token:

![](Attacking%20Session%20Tokens-20260914-102553.png)

![](Attacking%20Session%20Tokens-20260914-102700.png)

The session token is 32 characters long; thus, it seems infeasible to enumerate other users' valid sessions. However, let us send the login request multiple times and take note of the session tokens assigned by the web application, resulting in the following list of session tokens:

```txt
2c0c58b27c71a2ec5bf2b4b6e892b9f9
2c0c58b27c71a2ec5bf2b4546092b9f9
2c0c58b27c71a2ec5bf2b497f592b9f9
2c0c58b27c71a2ec5bf2b48bcf92b9f9
2c0c58b27c71a2ec5bf2b4735e92b9f9
```

Since 28 out of 32 characters are static, there are only four characters we need to enumerate to brute-force all existing active sessions, enabling us to hijack all active sessions.

Another vulnerable example would be an incrementing session identifier. For instance, consider the following capture of successive session tokens:

```txt
141233
141234
141237
141238
141240
```
## Attacking Predictable Session Tokens

The simplest form of predictable session tokens contains encoded data we can tamper with. For instance, consider the following session token:

![](Attacking%20Session%20Tokens-20260914-103018.png)

While this session token might seem random at first, a simple analysis reveals that it is base64-encoded data:

```sh
3kjS@htb[/htb]$ echo -n dXNlcj1odGItc3RkbnQ7cm9sZT11c2Vy | base64 -d

user=htb-stdnt;role=user
```

As we can see, the cookie contains information about the user and the role tied to the session. However, there is no security measure in place that prevents us from tampering with the data. We can forge our own session token by manipulating the data and base64-encoding it to match the expected format, enabling us to forge an admin cookie:

```sh
3kjS@htb[/htb]$ echo -n 'user=htb-stdnt;role=admin' | base64

dXNlcj1odGItc3RkbnQ7cm9sZT1hZG1pbg==
```

We can send this cookie to the web application to obtain administrative access:

![](Attacking%20Session%20Tokens-20260914-103118.png)

The same exploit works for cookies containing differently encoded data. We should also keep an eye out for data in hexadecimal encoding or URL encoding. For instance, a session token containing hex-encoded data might look like this:

![](Attacking%20Session%20Tokens-20260914-103130.png)

```sh
3kjS@htb[/htb]$ echo -n 'user=htb-stdnt;role=admin' | xxd -p

757365723d6874622d7374646e743b726f6c653d61646d696e
```
