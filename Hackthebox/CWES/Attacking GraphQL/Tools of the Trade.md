## GraphQL-Cop

We can use the tool [GraphQL-Cop](https://github.com/dolevf/graphql-cop), a security audit tool for GraphQL APIs. After cloning the GitHub repository and installing the required dependencies, we can run the `graphql-cop.py` Python script:

```sh
3kjS@htb[/htb]$ python3 graphql-cop.py  -v

version: 1.13
```

```sh
3kjS@htb[/htb]$ python3 graphql-cop/graphql-cop.py -t http://172.17.0.2/graphql
```
## InQL

[InQL](https://github.com/doyensec/inql) is a Burp extension we can install via the `BApp Store` in Burp. After a successful installation, an `InQL` tab is added in Burp.

![](Tools%20of%20the%20Trade-20260914-143655.png)

Furthermore, we can right-click on a GraphQL request and select `Extensions > InQL - GraphQL Scanner > Generate queries with InQL Scanner`:

![GraphQL request and response with menu options. Request: POST to /graphql. Menu: Extensions > InQL - GraphQL Scanner with options to generate queries, batch attack, or open in GraphiQL. Response: HTTP 200 OK, returns user with uuid "1" and username "htb-stdnt".](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/271/inql_2.png)

Afterward, InQL generates introspection information. The information regarding all mutations and queries is provided in the `InQL` tab for the scanned host:

![](Tools%20of%20the%20Trade-20260914-143806.png)

This is only a basic overview of InQL's functionality. Check out the official [GitHub repository](https://github.com/portswigger/inql) for more details.
