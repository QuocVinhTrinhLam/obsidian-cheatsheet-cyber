# [Hybrid Mode](Hackthebox/CWPE/Wi-Fi%20Password%20Cracking%20Techniques/Using%20Hashcat/Hybrid%20Mode.md)
### Crack the password for the WiFi network named "HTB-Hybrid" by using a wordlist entry followed by a 4-digit numeric pattern.

![](Screenshot%202026-10-04%20at%2014.30.53.png)

![](Screenshot%202026-10-04%20at%2014.32.26.png)

![](Screenshot%202026-10-04%20at%2014.35.00.png)

![](Screenshot%202026-10-04%20at%2014.36.40.png)

```sh
hashcat -m 22000 -a 6 HTB.22000 opt/wordlists.txt ?d?d?d?d
```

