## HTTPS Overview

![](Hypertext%20Transfer%20Protocol%20Secure%20(HTTPS)-20260907-110641.png)
![](Hypertext%20Transfer%20Protocol%20Secure%20(HTTPS)-20260907-110702.png)

Websites that enforce HTTPS can be identified through `https://` in their URL (e.g. [https://www.google.com](https://www.google.com/)), as well as the lock icon in the address bar of the web browser, to the left of the URL:
![](Hypertext%20Transfer%20Protocol%20Secure%20(HTTPS)-20260907-110713.png)
## HTTPS Flow

Let's look at how HTTPS operates at a high level:
![](Hypertext%20Transfer%20Protocol%20Secure%20(HTTPS)-20260907-110725.png)
## cURL for HTTPS

```shell
3kjS@htb[/htb]$ curl https://inlanefreight.com

curl: (60) SSL certificate problem: Invalid certificate chain
More details here: https://curl.haxx.se/docs/sslcerts.html
...SNIP...
```

```shell
3kjS@htb[/htb]$ curl -k https://www.inlanefreight.com

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
...SNIP...
```
