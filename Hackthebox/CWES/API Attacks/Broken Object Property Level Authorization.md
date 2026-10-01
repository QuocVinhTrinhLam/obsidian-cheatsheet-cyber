## Exposure of Sensitive Information Due to Incompatible Policies

The first endpoint we will be practicing against is vulnerable to [CWE-213](https://cwe.mitre.org/data/definitions/213.html), `Exposure of Sensitive Information Due to Incompatible Policies`.
### Scenario

The admin of `Inlanefreight E-Commerce Marketplace` has provided us with the credentials `htbpentester4@hackthebox.com:HTBPentester4`, wanting us to assess what API vulnerabilities the user can exploit with their assigned roles.

After invoking `/api/v1/authentication/customers/sign-in` to sign in as a customer and obtain a JWT, the `/api/v1/roles/current-user` endpoint shows that we have the roles `Suppliers_Get` and `Suppliers_GetAll`:

![](Broken%20Object%20Property%20Level%20Authorization-20260915-133142.png)

![](Broken%20Object%20Property%20Level%20Authorization-20260915-133214.png)
## Improperly Controlled Modification of Dynamically-Determined Object Attributes

The second API endpoint we will be practicing against is vulnerable to [CWE-915](https://cwe.mitre.org/data/definitions/915.html), `Improperly Controlled Modification of Dynamically-Determined Object Attributes`.
### Scenario

The admin of `Inlanefreight E-Commerce Marketplace` has provided us with the credentials `htbpentester6@pentestercompany.com:HTBPentester6`, wanting us to assess what API vulnerabilities the user can exploit with their assigned roles.

After invoking `/api/v1/authentication/suppliers/sign-in` to sign in as a Supplier and obtain a JWT, the `/api/v1/roles/current-user` endpoint shows that we have the roles `SupplierCompanies_Update` and `SupplierCompanies_Get`:

![](Broken%20Object%20Property%20Level%20Authorization-20260915-133337.png)

The `/api/v1/supplier-companies/current-user` endpoint shows that the supplier-company the currently authenticated supplier belongs to, 'PentesterCompany', has the `isExemptedFromMarketplaceFee` field set to `0`, which equates to `false`:

![](Broken%20Object%20Property%20Level%20Authorization-20260915-133408.png)

![](Broken%20Object%20Property%20Level%20Authorization-20260915-133503.png)

![](Broken%20Object%20Property%20Level%20Authorization-20260915-133529.png)

![](Broken%20Object%20Property%20Level%20Authorization-20260915-133545.png)
