## Filter/WAF Detection
![](Pasted%20image%2020260717141537.png)

```bash
127.0.0.1; whoami
```
Other than the IP (which we know is not blacklisted), we sent:

1. A semi-colon character `;`
2. A space character
3. A `whoami` command
## Blacklisted Characters

```php
$blacklist = ['&', '|', ';', ...SNIP...]; 
foreach ($blacklist as $character) { 
	if (strpos($_POST['ip'], $character) !== false) { 
		echo "Invalid input"; 
	} 
}
```
## Identifying Blacklisted Character
![](Pasted%20image%2020260717141717.png)
