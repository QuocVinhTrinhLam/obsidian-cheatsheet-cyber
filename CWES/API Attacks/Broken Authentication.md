## Improper Restriction of Excessive Authentication Attempts

The endpoint we will be practicing against is vulnerable to [CWE-307: Improper Restriction of Excessive Authentication Attempts](https://cwe.mitre.org/data/definitions/307.html).

![](Broken%20Authentication-20260914-164334.png)

When invoking the `/api/v1/customers/current-user` endpoint, we get back the information of our currently authenticated user:

![](Broken%20Authentication-20260914-164341.png)

The `/api/v1/roles/current-user` endpoint reveals that the user is assigned three roles: `Customers_UpdateByCurrentUser`, `Customers_Get`, and `Customers_GetAll`:

![](Broken%20Authentication-20260914-164548.png)

`Customers_GetAll` allows us to use the `/api/v1/customers` endpoint, which returns the records of all customers:

![](Broken%20Authentication-20260914-164555.png)

When we expand the `/api/v1/customers/current-user` `PATCH` endpoint, we discover that it allows us to update our information fields, including the account's password:

![](Broken%20Authentication-20260914-164604.png)

If we provide a weak password such as 'pass,' the API rejects the update, stating that passwords must be at least six characters long:

![](Broken%20Authentication-20260914-164607.png)

![](Broken%20Authentication-20260914-164618.png)

First, we need to obtain the (fail) message that the `/api/v1/authentication/customers/sign-in` endpoint returns when provided with incorrect credentials, which in this case is 'Invalid Credentials':

![](Broken%20Authentication-20260914-164706.png)

```sh
ffuf -w /opt/useful/seclists/Passwords/xato-net-10-million-passwords-10000.txt:PASS -w customerEmails.txt:EMAIL -u http://94.237.59.63:31874/api/v1/authentication/customers/sign-in -X POST -H "Content-Type: application/json" -d '{"Email": "EMAIL", "Password": "PASS"}' -fr "Invalid Credentials" -t 100
```
