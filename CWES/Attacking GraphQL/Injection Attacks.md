## SQL Injection

Using the introspection query discussed earlier and some trial-and-error, we can identify that the backend supports the following queries that require arguments:

- `post`
- `user`
- `postByAuthor`

![](Injection%20Attacks-20260914-140218.png)

After supplying the `author` argument, the query is executed successfully:

![](Injection%20Attacks-20260914-140224.png)

Let us move on to the `user` query. If we try the same payload there, the query still returns the previous result, indicating a SQL injection vulnerability:

![](Injection%20Attacks-20260914-140542.png)

If we simply inject a single quote, the response contains a SQL error, confirming the vulnerability:

![](Injection%20Attacks-20260914-140553.png)

To construct a UNION-based SQL injection payload, let us take another look at the results of the introspection query:

```url
https://graphql-kit.com/graphql-voyager/
```
![](Injection%20Attacks-20260914-140616.png)

As the GraphQL query only returns the first row, we will use the [GROUP_CONCAT](https://mariadb.com/kb/en/group_concat/) function to exfiltrate multiple rows at a time. This enables us to exfiltrate all table names in the current database with the following payload:

```graphql
{
  user(username: "x' UNION SELECT 1,2,GROUP_CONCAT(table_name),4,5,6 FROM information_schema.tables WHERE table_schema=database()-- -") {
    username
  }
}
```

The response contains all table names concatenated in the `username` field:

```graphql
{
  "data": {
    "user": {
      "username": "user,secret,post"
    }
  }
}
```
## Cross-Site Scripting (XSS)

![](Injection%20Attacks-20260914-140645.png)

![](Injection%20Attacks-20260914-140718.png)

However, if we attempt to trigger the URL from the corresponding GET parameter by accessing the URL `/post?id=<script>alert(1)</script>`, we can observe that the page simply breaks, and the XSS payload is not triggered.