|**Category**|**Description**|
|---|---|
|`Web Application Infrastructure`|Describes the structure of required components, such as the database, needed for the web application to function as intended. Since the web application can be set up to run on a separate server, it is essential to know which database server it needs to access.|
|`Web Application Components`|The components that make up a web application represent all the components that the web application interacts with. These are divided into the following three areas: `UI/UX`, `Client`, and `Server` components.|
|`Web Application Architecture`|Architecture comprises all the relationships between the various web application components.|
## Web Application Infrastructure

- `Client-Server`
- `One Server`
- `Many Servers - One Database`
- `Many Servers - Many Databases`
#### Client-Server

![](Web%20Application%20Layout-20260907-150141.png)
#### One Server

![](Web%20Application%20Layout-20260907-150431.png)
#### Many Servers - One Database

![](Web%20Application%20Layout-20260907-150442.png)
#### Many Servers - Many Databases

![](Web%20Application%20Layout-20260907-150455.png)
## Web Application Components

Each web application can have a different number of components. Nevertheless, all of the components of the models mentioned previously can be broken down to:

1. `Client`
2. `Server`
    - Webserver
    - Web Application Logic
    - Database
3. `Services` (Microservices)
    - 3rd Party Integrations
    - Web Application Integrations
4. `Functions` (Serverless)
## Web Application Architecture
|**Layer**|**Description**|
|---|---|
|`Presentation Layer`|Consists of UI process components that enable communication with the application and the system. These can be accessed by the client via the web browser and are returned in the form of HTML, JavaScript, and CSS.|
|`Application Layer`|This layer ensures that all client requests (web requests) are correctly processed. Various criteria are checked, such as authorization, privileges, and data passed on to the client.|
|`Data Layer`|The data layer works closely with the application layer to determine exactly where the required data is stored and can be accessed.|
![](Web%20Application%20Layout-20260907-150525.png)
