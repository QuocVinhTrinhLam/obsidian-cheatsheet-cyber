## Commands Blacklist

![](Pasted%20image%2020260717171054.png)

A basic command blacklist filter in `PHP` would look like the following:

```php
$blacklist = ['whoami', 'cat', ...SNIP...]; 
foreach ($blacklist as $word) { 
	if (strpos('$_POST['ip']', $word) !== false) { 
		echo "Invalid input"; 
	} 
}
```
## Linux & Windows

```shell
21y4d@htb[/htb]$ w'h'o'am'i 

21y4d
```
The same works with double-quotes as well:
```shell
21y4d@htb[/htb]$ w"h"o"am"i 

21y4d
```
![](Pasted%20image%2020260717172026.png)
## Linux Only

```bash
who$@ami 
w\ho\am\i
```
## Windows Only

```cmd
C:\htb> who^ami 
21y4d
```
