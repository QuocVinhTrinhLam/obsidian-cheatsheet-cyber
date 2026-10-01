# [Query Results](Query%20Results.md)
### What is the last name of the employee whose first name starts with "Bar" AND who was hired on 1990-01-01?

```shell
mysql -u root -p -h 154.57.164.79 -P 32176 --skip-ssl
```

```sql
select * from employees where hire_date = '1990-01-01';
```
![](Screenshot%202026-06-15%20at%2020.50.56.png)