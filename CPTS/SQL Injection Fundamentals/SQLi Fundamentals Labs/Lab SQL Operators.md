# [SQL Operators](SQL%20Operators.md)
### In the 'titles' table, what is the number of records WHERE the employee number is greater than 10000 OR their title does NOT contain 'engineer'?

```sql
SELECT COUNT(*) FROM titles WHERE emp_no > 10000 OR title NOT LIKE '%engineer%';
```
![](Screenshot%202026-06-15%20at%2021.16.42.png)