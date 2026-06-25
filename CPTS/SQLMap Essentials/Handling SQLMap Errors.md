## Display Errors
The first step is usually to switch the `--parse-errors`, to parse the DBMS errors (if any) and displays them as part of the program run:

![](Screenshot%202026-06-22%20at%2012.49.43.png)

With this option, SQLMap will automatically print the DBMS error, thus giving us clarity on what the issue may be so that we can properly fix it.
## Store the Traffic
The `-t` option stores the whole traffic content to an output file:

```shell
3kjS@htb[/htb]$ sqlmap -u "http://www.target.com/vuln.php?id=1" --batch -t /tmp/traffic.txt

...SNIP...

3kjS@htb[/htb]$ cat /tmp/traffic.txt
HTTP request [#1]: 
GET /?id=1 HTTP/1.1 
Host: www.example.com 
Cache-control: no-cache 
Accept-encoding: gzip,deflate 
Accept: */* User-agent: sqlmap/1.4.9 (http://sqlmap.org) 
Connection: close 

HTTP response [#1] (200 OK): 
Date: Thu, 24 Sep 2020 14:12:50 GMT 
Server: Apache/2.4.41 (Ubuntu) 
Vary: Accept-Encoding 
Content-Encoding: gzip 
Content-Length: 914 
Connection: close 
Content-Type: text/html; charset=UTF-8 
URI: http://www.example.com:80/?id=1 

<!DOCTYPE html> 
<html lang="en"> 
...SNIP...
```
