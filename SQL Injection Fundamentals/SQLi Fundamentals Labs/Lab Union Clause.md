# [Union Clause](Union%20Clause.md)
### Connect to the above MySQL server with the 'mysql' tool, and find the number of records returned when doing a 'Union' of all records in the 'employees' table and all records in the 'departments' table.

```shell
mysql -u root -p -h 154.57.164.74 -P 32546 --skip-ssl
```
![](Screenshot%202026-06-17%20at%2018.30.48.png)![](Screenshot%202026-06-17%20at%2018.31.00.png)

```sql
SELECT COUNT(*) FROM (
    ->     SELECT dept_no, dept_name FROM departments
    ->     UNION
    ->     SELECT emp_no, first_name FROM employees
    -> ) AS gop_bang;
```
![](Screenshot%202026-06-17%20at%2018.31.19.png)