## API Building Styles

Web APIs can be built using various architectural styles, including `REST`, `SOAP`, `GraphQL`, and `gRPC`, each with its own strengths and use cases:

- [Representational State Transfer](https://roy.gbiv.com/pubs/dissertation/fielding_dissertation.pdf#:~:text=This%20chapter%20introduces%20and%20elaborates%20the%20Representational%20State%20Transfer) (`REST`) is the most popular API style. It uses a `client-server` model where clients make requests to resources on a server using standard HTTP methods (`GET`, `POST`, `PUT`, `DELETE`). `RESTful` APIs are stateless, meaning each request contains all necessary information for the server to process it, and responses are typically serialized as JSON or XML.
- [Simple Object Access Protocol](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/) (`SOAP`) uses XML for message exchange between systems. `SOAP` APIs are highly standardized and offer comprehensive features for security, transactions, and error handling, but they are generally more complex to implement and use than `RESTful` APIs.
- [GraphQL](https://graphql.org/) is an alternative style that provides a more flexible and efficient way to fetch and update data. Instead of returning a fixed set of fields for each resource, `GraphQL` allows clients to specify exactly what data they need, reducing over-fetching and under-fetching of data. `GraphQL` APIs use a single endpoint and a strongly-typed query language to retrieve data.
- [gRPC](https://grpc.io/) is a newer style that uses [Protocol Buffers](https://protobuf.dev/) for message serialization, providing a high-performance, efficient way to communicate between systems. `gRPC` APIs can be developed in a variety of programming languages and are particularly useful for microservices and distributed systems.
# API Attacks

The very nature of APIs, facilitating data exchange and communication between diverse systems, introduces vulnerabilities, such as `Exposure of Sensitive Data`, `Authentication and Authorization Issues`, `Insufficient Rate Limiting`, `Improper Error Handling`, and various other security misconfigurations.
## OWASP Top 10 API Security Risks

|**Risk**|**Description**|
|---|---|
|[API1:2023 - Broken Object Level Authorization](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/)|The API allows authenticated users to access data they are not authorized to view.|
|[API2:2023 - Broken Authentication](https://owasp.org/API-Security/editions/2023/en/0xa2-broken-authentication/)|The authentication mechanisms of the API can be bypassed or circumvented, allowing unauthorized access.|
|[API3:2023 - Broken Object Property Level Authorization](https://owasp.org/API-Security/editions/2023/en/0xa3-broken-object-property-level-authorization/)|The API reveals sensitive data to authorized users that they should not access or permits them to manipulate sensitive properties.|
|[API4:2023 - Unrestricted Resource Consumption](https://owasp.org/API-Security/editions/2023/en/0xa4-unrestricted-resource-consumption/)|The API does not limit the amount of resources users can consume.|
|[API5:2023 - Broken Function Level Authorization](https://owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization/)|The API allows unauthorized users to perform authorized operations.|
|[API6:2023 - Unrestricted Access to Sensitive Business Flows](https://owasp.org/API-Security/editions/2023/en/0xa6-unrestricted-access-to-sensitive-business-flows/)|The API exposes sensitive business flows, leading to potential financial losses and other damages.|
|[API7:2023 - Server Side Request Forgery](https://owasp.org/API-Security/editions/2023/en/0xa7-server-side-request-forgery/)|The API does not validate requests adequately, allowing attackers to send malicious requests and interact with internal resources.|
|[API8:2023 - Security Misconfiguration](https://owasp.org/API-Security/editions/2023/en/0xa8-security-misconfiguration/)|The API suffers from security misconfigurations, including vulnerabilities that lead to Injection Attacks.|
|[API9:2023 - Improper Inventory Management](https://owasp.org/API-Security/editions/2023/en/0xa9-improper-inventory-management/)|The API does not properly and securely manage version inventory.|
|[API10:2023 - Unsafe Consumption of APIs](https://owasp.org/API-Security/editions/2023/en/0xaa-unsafe-consumption-of-apis/)|The API consumes another API unsafely, leading to potential security risks.|