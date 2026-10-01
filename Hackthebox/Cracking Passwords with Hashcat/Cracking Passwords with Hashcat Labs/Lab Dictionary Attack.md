# [Dictionary Attack](Dictionary%20Attack.md)
### Crack the following hash using the rockyou.txt wordlist: 0c352d5b2f45217c57bef9f8452ce376

![](Screenshot%202026-09-29%20at%2015.15.40.png)

```sh
hashcat -a 0 -m 0 hash /usr/share/wordlists/rockyou.txt
```

![](Screenshot%202026-09-29%20at%2015.18.12.png)