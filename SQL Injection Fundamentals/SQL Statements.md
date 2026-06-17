## INSERT Statement

```sql
INSERT INTO table_name VALUES (column1_value, column2_value, column3_value, ...);
```

```sql
mysql> INSERT INTO logins VALUES(1, 'admin', 'p@ssw0rd', '2020-07-02'); 

Query OK, 1 row affected (0.00 sec)
```

```sql
INSERT INTO table_name(column2, column3, ...) VALUES (column2_value, column3_value, ...);
```

We can do the same to insert values into the `logins` table:

```sql
mysql> INSERT INTO logins(username, password) VALUES('administrator', 'adm1n_p@ss'); 

Query OK, 1 row affected (0.00 sec)
```

```sql
mysql> INSERT INTO logins(username, password) VALUES ('john', 'john123!'), ('tom', 'tom123!'); 

Query OK, 2 rows affected (0.00 sec) Records: 2 Duplicates: 0 Warnings: 0
```
## SELECT Statement

```sql
SELECT * FROM table_name;
```

```sql
SELECT column1, column2 FROM table_name;
```

```sql
mysql> SELECT * FROM logins;
```
![](Screenshot%202026-06-15%20at%2020.29.14.png)
## DROP Statement

```sql
mysql> DROP TABLE logins; 

Query OK, 0 rows affected (0.01 sec) 


mysql> SHOW TABLES; 

Empty set (0.00 sec)
```
## ALTER Statement

```sql
mysql> ALTER TABLE logins ADD newColumn INT; 

Query OK, 0 rows affected (0.01 sec)
```

To rename a column, we can use `RENAME COLUMN`:

```sql
mysql> ALTER TABLE logins RENAME COLUMN newColumn TO newerColumn; 

Query OK, 0 rows affected (0.01 sec)
```

We can also change a column's datatype with `MODIFY`:

```sql
mysql> ALTER TABLE logins MODIFY newerColumn DATE; 

Query OK, 0 rows affected (0.01 sec)
```

Finally, we can drop a column using `DROP`:

```sql
mysql> ALTER TABLE logins DROP newerColumn; 

Query OK, 0 rows affected (0.01 sec)
```
## UPDATE Statement

```sql
UPDATE table_name SET column1=newvalue1, column2=newvalue2, ... WHERE <condition>;
```

```sql
mysql> UPDATE logins SET password = 'change_password' WHERE id > 1; 

Query OK, 3 rows affected (0.00 sec) Rows matched: 3 Changed: 3 Warnings: 0
```
![](Screenshot%202026-06-15%20at%2020.31.38.png)
