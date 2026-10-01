# [Cracking Common Hashes](Cracking%20Common%20Hashes.md)
### Crack the following hash: 7106812752615cdfe427e01b98cd4083

![](Screenshot%202026-09-29%20at%2016.37.14.png)

```sh
hashcat -a 0 -m 1000 -g 1000 7106812752615cdfe427e01b98cd4083 /usr/share/wordlists/rockyou.txt
```

![](Screenshot%202026-09-29%20at%2016.41.11.png)