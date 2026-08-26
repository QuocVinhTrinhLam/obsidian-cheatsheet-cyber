# [Credential Hunting](CPTS/Linux%20Privilege%20Escalation/Information%20Gathering/Credential%20Hunting.md)
### Find the WordPress database password.

```shell
find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null | grep wp
```
![](Screenshot%202026-08-04%20at%2013.05.37.png)

```shell
cat /var/www/html/wp-config.php | grep 'DB_USER\|DB_PASSWORD'
```
![](Screenshot%202026-08-04%20at%2013.07.01.png)
