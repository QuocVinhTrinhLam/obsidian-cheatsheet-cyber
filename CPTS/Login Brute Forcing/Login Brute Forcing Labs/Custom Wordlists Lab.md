# [Custom Wordlists](Custom%20Wordlists.md)
### After successfully brute-forcing, and then logging into the target, what is the full flag you find?

Create password file with `CUPP`
![](Screenshot%202026-06-14%20at%2016.22.21.png)

Create username file with `username-anarchy`
![](Screenshot%202026-06-14%20at%2016.23.48.png)

Now i gonna filter the password to match the policy

```shell
grep -E '^.{6,}$' jane.txt | grep -E '[A-Z]' | grep -E '[a-z]' | grep -E '[0-9]' | grep -E '([!@#$%^&*].*){2,}' > jane-filtered.txt
```
![](Screenshot%202026-06-14%20at%2016.25.14.png)

Lets start attack, the tool i chose is `hydra` 
![](Screenshot%202026-06-14%20at%2016.28.18.png)

Pwned !
![](Screenshot%202026-06-14%20at%2016.29.03.png)