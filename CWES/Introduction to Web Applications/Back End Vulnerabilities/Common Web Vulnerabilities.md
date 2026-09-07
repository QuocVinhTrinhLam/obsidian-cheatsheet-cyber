## Broken Authentication/Access Control

For example, `College Management System 1.2` has a simple [Auth Bypass](https://www.exploit-db.com/exploits/47388) vulnerability that allows us to login without having an account, by inputting the following for the email field: `' or 0=0 #`, and using any password with it.
## Malicious File Upload

For example, the WordPress Plugin `Responsive Thumbnail Slider 1.0` can be exploited to upload any arbitrary file, including malicious scripts, by uploading a file with a double extension (i.e. `shell.php.jpg`). There's even a [Metasploit Module](https://www.rapid7.com/db/modules/exploit/multi/http/wp_responsive_thumbnail_slider_upload/) that allows us to exploit this vulnerability easily.
## Command Injection

For example, the WordPress Plugin `Plainview Activity Monitor 20161228` has a [vulnerability](https://www.exploit-db.com/exploits/45274) that allows attackers to inject their command in the `ip` value, by simply adding `| COMMAND...` after the `ip` value.
## SQL Injection (SQLi)

For example, in the `database` section, we saw an example of how a web application would use user-input to search within a certain table, with the following line of code:

```php
$query = "select * from users where name like '%$searchInput%'";
```

The same previous `College Management System 1.2` suffers from a SQL injection [vulnerability](https://www.exploit-db.com/exploits/47388), in which we can execute another `SQL` query that always returns `true`, meaning we successfully authenticated, which allows us to log in to the application. We can use the same vulnerability to retrieve data from the database or even gain control over the hosting server.