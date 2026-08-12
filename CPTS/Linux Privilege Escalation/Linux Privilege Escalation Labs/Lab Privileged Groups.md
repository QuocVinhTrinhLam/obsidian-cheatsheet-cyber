# [Privileged Groups](Privileged%20Groups.md)
### Use the privileged group rights of the secaudit user to locate a flag.

```shell
id

find / -group adm 2</dev/null
```
![](Screenshot%202026-08-05%20at%2014.29.11.png)

```shell
grep -R "HTB" /var/log 2>/dev/null
```
![](Screenshot%202026-08-05%20at%2014.33.04.png)

```shell
cat /var/log/apache2/access.log | grep -I flag
```
![](Screenshot%202026-08-05%20at%2014.39.28.png)
