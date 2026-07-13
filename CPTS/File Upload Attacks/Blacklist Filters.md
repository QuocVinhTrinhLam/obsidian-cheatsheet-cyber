## Blacklisting Extensions
![](Pasted%20image%2020260708143326.png)

```php
$fileName = basename($_FILES["uploadFile"]["name"]); 
$extension = pathinfo($fileName, PATHINFO_EXTENSION); 
$blacklist = array('php', 'php7', 'phps'); 

if (in_array($extension, $blacklist)) { 
	echo "File type not allowed"; 
	die(); 
}
```
## Fuzzing Extensions
![](Pasted%20image%2020260708145924.png)
![](Pasted%20image%2020260708145928.png)
## Non-Blacklisted Extensions
![](Pasted%20image%2020260708145954.png)
![](Pasted%20image%2020260708145957.png)
