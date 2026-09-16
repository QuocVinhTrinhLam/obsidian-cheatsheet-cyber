## Scenario

In the previous section, we exploited a `BFLA` vulnerability and gained access to product discount data. This data exposure also leads to `Unrestricted Access to Sensitive Business Flows` because it allows us to know the dates when supplier companies will discount their products and the corresponding discount rates. For example, if we want to buy the product with ID `a923b706-0aaa-49b2-ad8d-21c97ff6fac7`, we should purchase it between `2023-03-15` and `2023-09-15` because it will be 70% off its original price:

![](Unrestricted%20Access%20to%20Sensitive%20Business%20Flows-20260915-151021.png)
