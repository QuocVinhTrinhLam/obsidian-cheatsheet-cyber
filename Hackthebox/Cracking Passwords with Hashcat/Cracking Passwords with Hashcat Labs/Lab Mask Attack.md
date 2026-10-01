# [Mask Attack](Mask%20Attack.md)
### Crack the following MD5 hash using a mask attack: 50a742905949102c961929823a2e8ca0. Use the following mask: -1 02 'HASHCAT?l?l?l?l?l20?1?d'

```sh
hashcat -a 3 -m 0 hash -1 02 'HASHCAT?l?l?l?l?l20?1?d'
```

![](Screenshot%202026-09-29%20at%2015.39.48.png)