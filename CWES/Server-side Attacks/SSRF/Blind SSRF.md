## Identifying Blind SSRF

```sh
3kjS@htb[/htb]$ nc -lnvp 8000

listening on [any] 8000 ...
connect to [172.17.0.1] from (UNKNOWN) [172.17.0.2] 32928
GET /index.php HTTP/1.1
Host: 172.17.0.1:8000
Accept: */*
```

![](Blind%20SSRF-20260913-134121.png)
## Exploiting Blind SSRF

In this case, the web application responds with `Something went wrong!` for closed ports:

![](Blind%20SSRF-20260913-134220.png)

However, if a port is open and responds with a valid HTTP response, we get a different error message:

![](Blind%20SSRF-20260913-134235.png)

For instance, we are unable to identify the running MySQL service using this technique:

![](Blind%20SSRF-20260913-134303.png)

Furthermore, although we cannot read local files as before, we can still use the same technique to identify existing files on the filesystem. That is because the error message is different for existing and non-existing files, just like it differs for open and closed ports:

![](Blind%20SSRF-20260913-134342.png)

For invalid files, the error message is different:

![](Blind%20SSRF-20260913-134357.png)