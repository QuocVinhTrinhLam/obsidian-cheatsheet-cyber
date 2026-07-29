## Discovery/Footprinting

```shell
User-agent: * 
Disallow: /wp-admin/ 
Allow: /wp-admin/admin-ajax.php 
Disallow: /wp-content/uploads/wpforms/ 

Sitemap: https://inlanefreight.local/wp-sitemap.xml
```
![](Pasted%20image%2020260727132912.png)
## Enumeration

```shell
3kjS@htb[/htb]$ curl -s http://blog.inlanefreight.local | grep WordPress 

<meta name="generator" content="WordPress 5.8" /
```

```shell
3kjS@htb[/htb]$ curl -s http://blog.inlanefreight.local/ | grep themes 

<link rel='stylesheet' id='bootstrap-css' href='http://blog.inlanefreight.local/wp-content/themes/business-gravity/assets/vendors/bootstrap/css/bootstrap.min.css' type='text/css' media='all' />
```

```shell
3kjS@htb[/htb]$ curl -s http://blog.inlanefreight.local/ | grep plugins
```
![](Screenshot%202026-07-27%20at%2013.31.03.png)

```shell
3kjS@htb[/htb]$ curl -s http://blog.inlanefreight.local/?p=1 | grep plugins
```
![](Screenshot%202026-07-27%20at%2013.32.41.png)
## Enumerating Users
![](Pasted%20image%2020260727133255.png)
![](Pasted%20image%2020260727133309.png)
## WPScan

```shell
3kjS@htb[/htb]$ sudo gem install wpscan
```

```shell
3kjS@htb[/htb]$ sudo wpscan --url http://blog.inlanefreight.local --enumerate --api-token dEOFB<SNIP>
```
