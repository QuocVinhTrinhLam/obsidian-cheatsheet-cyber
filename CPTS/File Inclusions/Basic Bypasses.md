## Non-Recursive Path Traversal Filters

```php
$language = str_replace('../', '', $_GET['language']);
```
![](Screenshot%202026-06-30%20at%2014.22.09.png)
![](Screenshot%202026-06-30%20at%2014.22.50.png)
## Encoding
![](Pasted%20image%2020260630142440.png)
![](Screenshot%202026-06-30%20at%2014.24.58.png)
## Approved Paths

```php
if(preg_match('/^\.\/languages\/.+$/', $_GET['language'])) { 
	include($_GET['language']); 
} else { 
	echo 'Illegal path specified!'; 
}
```
![](Screenshot%202026-06-30%20at%2014.26.48.png)
