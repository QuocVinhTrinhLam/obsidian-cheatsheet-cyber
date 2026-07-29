## Discovery/Footprinting

```shell
3kjS@htb[/htb]$ curl -s http://drupal.inlanefreight.local | grep Drupal 

<meta name="Generator" content="Drupal 8 (https://www.drupal.org)" /> 
	<span>Powered by <a href="https://www.drupal.org">Drupal</a></span>
```

```url
http://drupal.inlanefreight.local/node/1
```
![](Pasted%20image%2020260729120004.png)
## Enumeration

```shell
3kjS@htb[/htb]$ curl -s http://drupal-acc.inlanefreight.local/CHANGELOG.txt | grep -m2 "" 

Drupal 7.57, 2018-02-21
```

```shell
3kjS@htb[/htb]$ curl -s http://drupal.inlanefreight.local/CHANGELOG.txt

<!DOCTYPE html><html><head><title>404 Not Found</title></head><body><h1>Not Found</h1><p>The requested URL "http://drupal.inlanefreight.local/CHANGELOG.txt" was not found on this server.</p></body></html>
```

Let's run a scan against the `http://drupal.inlanefreight.local` host.

```shell
3kjS@htb[/htb]$ droopescan scan drupal -u http://drupal.inlanefreight.local 

[+] Plugins found: 
	php http://drupal.inlanefreight.local/modules/php/ 
		http://drupal.inlanefreight.local/modules/php/LICENSE.txt 
		
[+] No themes found. 

[+] Possible version(s): 
	8.9.0 
	8.9.1 
	
[+] Possible interesting urls found: 
	Default admin - http://drupal.inlanefreight.local/user/login 
	
[+] Scan finished (0:03:19.199526 elapsed)
```
