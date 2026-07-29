
```shell
3kjS@htb[/htb]$ curl -s https://developer.joomla.org/stats/cms_version | python3 -m json.tool
```
![](Screenshot%202026-07-28%20at%2015.14.14.png)
## Discovery/Footprinting

```shell
3kjS@htb[/htb]$ curl -s http://dev.inlanefreight.local/ | grep Joomla 
	
	<meta name="generator" content="Joomla! - Open Source Content Management" /> 
	
<SNIP>
```

The `robots.txt` file for a Joomla site will often look like this:
![](Screenshot%202026-07-28%20at%2015.20.39.png)

```shell
3kjS@htb[/htb]$ curl -s http://dev.inlanefreight.local/README.txt | head -n 5
```

```shell
3kjS@htb[/htb]$ curl -s http://dev.inlanefreight.local/administrator/manifests/files/joomla.xml | xmllint --format -
```
![](Screenshot%202026-07-28%20at%2015.21.27.png)
The `cache.xml` file can help to give us the approximate version. It is located at `plugins/system/cache/cache.xml`.
## Enumeration

```shell
3kjS@htb[/htb]$ sudo pip3 install droopescan
```

```shell
3kjS@htb[/htb]$ droopescan scan joomla --url http://dev.inlanefreight.local/
```
![](Screenshot%202026-07-28%20at%2015.23.45.png)

```shell
3kjS@htb[/htb]$ python2 -m pip install bs4
```

```shell
3kjS@htb[/htb]$ python2 joomlascan.py -u http://dev.inlanefreight.local
```

The default administrator account on Joomla installs is `admin`, but the password is set at install time, so the only way we can hope to get into the admin back-end is if the account is set with a very weak/common password and we can get in with some guesswork or light brute-forcing. We can use this [script](https://github.com/ajnik/joomla-bruteforce) to attempt to brute force the login.

```shell
3kjS@htb[/htb]$ sudo python3 joomla-brute.py -u http://dev.inlanefreight.local -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin 

admin:admin
```
