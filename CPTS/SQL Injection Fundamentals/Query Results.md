## Sorting Results

```sql
mysql> SELECT * FROM logins ORDER BY password;
```
![](Screenshot%202026-06-15%20at%2020.41.31.png)

```sql
mysql> SELECT * FROM logins ORDER BY password DESC;
```
![](Screenshot%202026-06-15%20at%2020.42.28.png)

```sql
mysql> SELECT * FROM logins ORDER BY password DESC, id ASC;
```
![](Screenshot%202026-06-15%20at%2020.43.31.png)
## LIMIT results

```sql
mysql> SELECT * FROM logins LIMIT 2;
```
![](Screenshot%202026-06-15%20at%2020.43.53.png)

If we wanted to LIMIT results with an offset, we could specify the offset before the LIMIT count:

```sql
mysql> SELECT * FROM logins LIMIT 1, 2;
```
![](Screenshot%202026-06-15%20at%2020.44.15.png)
## WHERE Clause

```sql
SELECT * FROM table_name WHERE <condition>;
```

```sql
mysql> SELECT * FROM logins WHERE id > 1;
```
![](Screenshot%202026-06-15%20at%2020.44.39.png)

```sql
mysql> SELECT * FROM logins where username = 'admin';
```
![](Screenshot%202026-06-15%20at%2020.45.13.png)
## LIKE Clause

```sql
mysql> SELECT * FROM logins WHERE username LIKE 'admin%';
```
![](Screenshot%202026-06-15%20at%2020.45.30.png)

```sql
mysql> SELECT * FROM logins WHERE username like '___';
```
![](Screenshot%202026-06-15%20at%2020.46.07.png)