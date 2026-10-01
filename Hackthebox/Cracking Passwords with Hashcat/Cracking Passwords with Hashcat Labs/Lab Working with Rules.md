# [Working with Rules](Working%20with%20Rules.md)
### Crack the following SHA1 hash using the techniques taught for generating a custom rule: 46244749d1e8fb99c37ad4f14fccb601ed4ae283. Modify the example rule in the beginning of the section to append 2020 to the end of each password attempt.

```sh
hashcat -a 0 -m 100 46244749d1e8fb99c37ad4f14fccb601ed4ae283 /usr/share/wordlists/rockyou.txt -r rule.txt
```

![](Screenshot%202026-09-29%20at%2016.24.52.png)