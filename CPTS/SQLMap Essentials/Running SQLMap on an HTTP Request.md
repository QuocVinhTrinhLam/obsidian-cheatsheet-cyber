## Curl Commands
![](Pasted%20image%2020260622120617.png)

```shell
3kjS@htb[/htb]$ sqlmap 'http://www.example.com/?id=1' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0' -H 'Accept: image/webp,*/*' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Connection: keep-alive' -H 'DNT: 1'
```
## GET/POST Requests

```shell
3kjS@htb[/htb]$ sqlmap 'http://www.example.com/' --data 'uid=1&name=test'
```

```shell
3kjS@htb[/htb]$ sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'
```
## Full HTTP Requests
![](Pasted%20image%2020260622120931.png)

An example of an HTTP request captured with `Burp` would look like:

```http
GET /?id=1 HTTP/1.1 
Host: www.example.com 
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0 
Accept:  text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5 Accept-Encoding: gzip, deflate 
Connection: close 
Upgrade-Insecure-Requests: 1 
DNT: 1 
If-Modified-Since: Thu, 17 Oct 2019 07:18:26 GMT 
If-None-Match: "3147526947" 
Cache-Control: max-age=0
```

```shell
3kjS@htb[/htb]$ sqlmap -r req.txt
```
![](Screenshot%202026-06-22%20at%2012.11.05.png)
## Custom SQLMap Requests

```shell
3kjS@htb[/htb]$ sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
```

The same effect can be done with the usage of option `-H/--header`:

```shell
3kjS@htb[/htb]$ sqlmap ... -H='Cookie:PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
```

```shell
3kjS@htb[/htb]$ sqlmap -u www.target.com --data='id=1' --method PUT
```
## Custom HTTP Requests
![](Screenshot%202026-06-22%20at%2012.17.10.png)

```shell
3kjS@htb[/htb]$ sqlmap -r req.txt
```
![](Screenshot%202026-06-22%20at%2012.17.26.png)
