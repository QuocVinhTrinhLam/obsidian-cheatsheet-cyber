## Content-Type
![](Pasted%20image%2020260710132247.png)

```php
$type = $_FILES['uploadFile']['type']; 
	if (!in_array($type, array('image/jpg', 'image/jpeg', 'image/png', 'image/gif'))) { 
	echo "Only images are allowed"; 
	die(); 
}
```

```shell
3kjS@htb[/htb]$ wget https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Discovery/Web-Content/web-all-content-types.txt 
3kjS@htb[/htb]$ cat web-all-content-types.txt | grep 'image/' > image-content-types.txt
```
![](Pasted%20image%2020260710132536.png)
## MIME-Type

```shell
3kjS@htb[/htb]$ echo "this is a text file" > text.jpg 
3kjS@htb[/htb]$ file text.jpg 
text.jpg: ASCII text
```

```shell
3kjS@htb[/htb]$ echo "GIF8" > text.jpg 
3kjS@htb[/htb]$ file text.jpg 
text.jpg: GIF image data
```

```php
$type = mime_content_type($_FILES['uploadFile']['tmp_name']); 
	
if (!in_array($type, array('image/jpg', 'image/jpeg', 'image/png', 'image/gif'))) { 
	echo "Only images are allowed"; 
	die(); 
}
```
![](Pasted%20image%2020260710132852.png)
![](Pasted%20image%2020260710132909.png)
![](Pasted%20image%2020260710132913.png)
