## AND Operator

```sql
condition1 AND condition2
```

```sql
mysql> SELECT 1 = 1 AND 'test' = 'test';
```
![](Screenshot%202026-06-15%20at%2020.52.35.png)

```sql
mysql> SELECT 1 = 1 AND 'test' = 'abc';
```
![](Screenshot%202026-06-15%20at%2020.52.46.png)
## OR Operator

```sql
mysql> SELECT 1 = 1 OR 'test' = 'abc';
```
![](Screenshot%202026-06-15%20at%2020.54.53.png)

```sql
mysql> SELECT 1 = 2 OR 'test' = 'abc';
```
![](Screenshot%202026-06-15%20at%2020.55.05.png)
## NOT Operator

```sql
mysql> SELECT NOT 1 = 1;
```
![](Screenshot%202026-06-15%20at%2020.55.33.png)

```sql
mysql> SELECT NOT 1 = 2;
```
![](Screenshot%202026-06-15%20at%2020.55.43.png)
## Symbol Operators

```sql
mysql> SELECT 1 = 1 && 'test' = 'abc';
```
![](Screenshot%202026-06-15%20at%2020.56.35.png)

```sql
mysql> SELECT 1 = 1 || 'test' = 'abc';
```
![](Screenshot%202026-06-15%20at%2020.56.45.png)

```sql
mysql> SELECT 1 != 1;
```
![](Screenshot%202026-06-15%20at%2020.56.55.png)
## Operators in queries

```sql
mysql> SELECT * FROM logins WHERE username != 'john';
```
![](Screenshot%202026-06-15%20at%2020.58.56.png)

```sql
mysql> SELECT * FROM logins WHERE username != 'john' AND id > 1;
```
![](Screenshot%202026-06-15%20at%2020.59.08.png)
## Multiple Operator Precedence

- Division (`/`), Multiplication (`*`), and Modulus (`%`)
- Addition (`+`) and subtraction (`-`)
- Comparison (`=`, `>`, `<`, `<=`, `>=`, `!=`, `LIKE`)
- NOT (`!`)
- AND (`&&`)
- OR (`||`)

```sql
SELECT * FROM logins WHERE username != 'tom' AND id > 3 - 2;
```

```sql
mysql> select * from logins where username != 'tom' AND id > 3 - 2;
```
![](Screenshot%202026-06-15%20at%2021.00.24.png)
