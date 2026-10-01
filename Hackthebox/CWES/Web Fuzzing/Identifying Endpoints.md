## REST

Endpoints in REST APIs are structured as URLs representing the resources you want to access or manipulate. For example:

- `/users` - Represents a collection of user resources.
- `/users/123` - Represents a specific user with the ID 123.
- `/products` - Represents a collection of product resources.
- `/products/456` - Represents a specific product with the ID 456.

|Parameter Type|Description|Example|
|---|---|---|
|Query Parameters|Appended to the endpoint URL after a question mark (`?`). Used for filtering, sorting, or pagination.|`/users?limit=10&sort=name`|
|Path Parameters|Embedded directly within the endpoint URL. Used to identify specific resources.|`/products/{id}pen_spark`|
|Request Body Parameters|Sent in the body of POST, PUT, or PATCH requests. Used to create or update resources.|`{ "name": "New Product", "price": 99.99 }`|
### Discovering REST Endpoints and Parameters

Discovering the available endpoints and parameters of a REST API can be accomplished through several methods:

1. `API Documentation`: The most reliable way to understand an API is to refer to its official documentation. This documentation often includes a list of available endpoints, their parameters, expected request/response formats, and example usage. Look for specifications like Swagger (OpenAPI) or RAML, which provide machine-readable API descriptions.
2. `Network Traffic Analysis`: If documentation is not available or incomplete, you can analyze network traffic to observe how the API is used. Tools like Burp Suite or your browser's developer tools allow you to intercept and inspect API requests and responses, revealing endpoints, parameters, and data formats.
3. `Parameter Name Fuzzing`: Similar to fuzzing for directories and files, you can use the same tools and techniques to fuzz for parameter names within API requests. Tools like `ffuf` and `wfuzz`, combined with appropriate wordlists, can be used to discover hidden or undocumented parameters. This can be particularly useful when dealing with APIs that lack comprehensive documentation.
## SOAP

Imagine a SOAP API for a library that offers a book search service. The WSDL file might define an operation called `SearchBooks` with the following input parameters:

- `keywords` (string): The search terms to use.
- `author` (string): The name of the author (optional).
- `genre` (string): The genre of the book (optional).

A sample SOAP request to this API might look like this:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:lib="http://example.com/library">
   <soapenv:Header/>
   <soapenv:Body>
      <lib:SearchBooks>
         <lib:keywords>cybersecurity</lib:keywords>
         <lib:author>Dan Kaminsky</lib:author>
      </lib:SearchBooks>
   </soapenv:Body>
</soapenv:Envelope>
```

In this request:

- The `keywords` parameter is set to "cybersecurity" to search for books on that topic.
- The `author` parameter is set to "Dan Kaminsky" to further refine the search.
- The `genre` parameter is not included, meaning the search will not be filtered by genre.

The SOAP response would likely contain a list of books matching the search criteria, formatted according to the WSDL definition.
### Discovering SOAP Endpoints and Parameters

To identify the available endpoints (operations) and parameters for a SOAP API, you can utilize the following methods:

1. `WSDL Analysis`: The WSDL file is the most valuable resource for understanding a SOAP API. It describes:
    
    - Available operations (endpoints)
    - Input parameters for each operation (message types, elements, and attributes)
    - Output parameters for each operation (response message types)
    - Data types used for parameters (e.g., strings, integers, complex types)
    - The location (URL) of the SOAP endpoint
    
      
    You can analyze the WSDL file manually or use tools designed to parse and visualize WSDL structures.
2. `Network Traffic Analysis`: Similar to REST APIs, you can intercept and analyze SOAP traffic to observe the requests and responses between clients and the server. Tools like Wireshark or tcpdump can capture SOAP traffic, allowing you to examine the structure of SOAP messages and extract information about endpoints and parameters.
3. `Fuzzing for Parameter Names and Values`: While SOAP APIs typically have a well-defined structure, fuzzing can still be helpful in uncovering hidden or undocumented operations or parameters. You can use fuzzing tools to send malformed or unexpected values within SOAP requests and see how the server responds.
## Identifying GraphQL API Endpoints and Parameters
### GraphQL Queries

|Component|Description|Example|
|---|---|---|
|Field|Represents a specific piece of data you want to retrieve (e.g., name, email).|`name`, `email`|
|Relationship|Indicates a connection between different types of data (e.g., a user's posts).|`posts`|
|Nested Object|A field that returns another object, allowing you to traverse deeper into the data graph.|`posts { title, body }`|
|Argument|Modifies the behavior of a query or field (e.g., filtering, sorting, pagination).|`posts(limit: 5)` (retrieves the first 5 posts of a user)|

```graphql
query {
  user(id: 123) {
    name
    email
    posts(limit: 5) {
      title
      body
    }
  }
}
```
### GraphQL Mutations

|Component|Description|Example|
|---|---|---|
|Operation|The action to perform (e.g., createPost, updateUser, deleteComment).|`createPost`|
|Argument|Input data required for the operation (e.g., title and body for a new post).|`title: "New Post", body: "This is the content of the new post"`|
|Selection|Fields you want to retrieve in the response after the mutation completes (e.g., id, title of new post).|`id`, `title`|

```graphql
mutation {
  createPost(title: "New Post", body: "This is the content of the new post") {
    id
    title
  }
}
```
### Discovering Queries and Mutations

There are a few ways to discover GraphQL Queries and Mutations:

1. `Introspection`: GraphQL's introspection system is a powerful tool for discovery. By sending an introspection query to the GraphQL endpoint, you can retrieve a complete schema describing the API's capabilities. This includes available types, fields, queries, mutations, and arguments. Tools and IDEs can leverage this information to offer auto-completion, validation, and documentation for your GraphQL queries.
2. `API Documentation`: Well-documented GraphQL APIs provide comprehensive guides and references alongside introspection. These typically explain the purpose and usage of different queries and mutations, offer examples of valid structures, and detail input arguments and response formats. Tools like GraphiQL or GraphQL Playground, often bundled with GraphQL servers, provide an interactive environment for exploring the schema and experimenting with queries.
3. `Network Traffic Analysis`: Like REST and SOAP, analyzing network traffic can yield insights into GraphQL API structure and usage. By capturing and inspecting requests and responses sent to the graphql endpoint, you can observe real-world queries and mutations. This helps you understand the expected format of requests and the types of data returned, aiding in tailored fuzzing efforts.
