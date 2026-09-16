## Exposure of Sensitive Information to an Unauthorized Actor

The endpoint we will be practicing against is vulnerable to [CWE-200: Exposure of Sensitive Information to an Unauthorized Actor](https://cwe.mitre.org/data/definitions/200.html).
### Scenario

![](Broken%20Function%20Level%20Authorization-20260915-150213.png)

![](Broken%20Function%20Level%20Authorization-20260915-150223.png)

Despite not having any roles, if we attempt to invoke the `/api/v1/products/discounts` endpoint, we notice that it returns data containing all the discounts for products:

![](Broken%20Function%20Level%20Authorization-20260915-150226.png)
