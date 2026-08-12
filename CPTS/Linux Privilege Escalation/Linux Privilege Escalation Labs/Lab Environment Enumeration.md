# [Environment Enumeration](Environment%20Enumeration.md)
### Enumerate the Linux environment and look for interesting files that might contain sensitive data. Submit the flag as the answer.

```shell
find / -type f -exec grep -H "HTB{" {} \; 2>/dev/null
```
![](Screenshot%202026-08-04%20at%2012.47.48.png)
