## Workflow

![](Web%20Servers-20260907-162417.png)

|Code|Description|
|---|---|
|**Successful responses**||
|`200 OK`|The request has succeeded|
|**Redirection messages**||
|`301 Moved Permanently`|The URL of the requested resource has been changed permanently|
|`302 Found`|The URL of the requested resource has been changed temporarily|
|**Client error responses**||
|`400 Bad Request`|The server could not understand the request due to invalid syntax|
|`401 Unauthorized`|Unauthenticated attempt to access page|
|`403 Forbidden`|The client does not have access rights to the content|
|`404 Not Found`|The server can not find the requested resource|
|`405 Method Not Allowed`|The request method is known by the server but has been disabled and cannot be used|
|`408 Request Timeout`|This response is sent on an idle connection by some servers, even without any previous request by the client|
|**Server error responses**||
|`500 Internal Server Error`|The server has encountered a situation it doesn't know how to handle|
|`502 Bad Gateway`|The server, while working as a gateway to get a response needed to handle the request, received an invalid response|
|`504 Gateway Timeout`|The server is acting as a gateway and cannot get a response in time|

The following shows an example of requesting a page in a Linux terminal using the [cURL](https://en.wikipedia.org/wiki/CURL) utility, and receiving the server response while using the `-I` flag, which displays the headers:

```shell
3kjS@htb[/htb]$ curl -I https://academy.hackthebox.com

HTTP/2 200
date: Tue, 15 Dec 2020 19:54:29 GMT
content-type: text/html; charset=UTF-8
...SNIP...
```

While this `cURL` command example shows us the source code of the webpage:

```shell
3kjS@htb[/htb]$ curl https://academy.hackthebox.com

<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<title>Cyber Security Training : HTB Academy</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
## Apache
![](Web%20Servers-20260907-162519.png)
`Apache` is an open-source project, and community users can access its source code to fix issues and look for vulnerabilities. It is well-maintained and regularly patched against vulnerabilities to keep it safe against exploitation. Furthermore, it is very well documented, making using and configuring different parts of the webserver relatively easy. `Apache` is commonly used by startups and smaller companies, as it is straightforward to develop for. Still, some big companies utilize Apache, including:

|`Apple`|`Adobe`|`Baidu`|
|---|---|---|
## NGINX
![](Web%20Servers-20260907-162542.png)
[NGINX](https://www.nginx.com/) is the second most common web server on the internet, hosting roughly `30%` of all internet websites. `NGINX` focuses on serving many concurrent web requests with relatively low memory and CPU load by utilizing an async architecture to do so. This makes `NGINX` a very reliable web server for popular web applications and top businesses worldwide, which is why it is the most popular web server among high traffic websites, with around 60% of the top 100,000 websites using `NGINX`.

`NGINX` is also free and open-source, which gives all the same benefits previously mentioned, like security and reliability. Some popular websites that utilize `NGINX` include:

|`Google`|`Facebook`|`Twitter`|`Cisco`|`Intel`|`Netflix`|`HackTheBox`|
|---|---|---|---|---|---|---|
## IIS
![](Web%20Servers-20260907-162554.png)

[IIS (Internet Information Services)](https://en.wikipedia.org/wiki/Internet_Information_Services) is the third most common web server on the internet, hosting around `15%` of all internet web sites. `IIS` is developed and maintained by Microsoft and mainly runs on Microsoft Windows Servers. `IIS` is usually used to host web applications developed for the Microsoft .NET framework, but can also be used to host web applications developed in other languages like `PHP`, or host other types of services like `FTP`.

| `Microsoft` | `Office365` | `Skype` | `Stack Overflow` | `Dell` |
| ----------- | ----------- | ------- | ---------------- | ------ |
