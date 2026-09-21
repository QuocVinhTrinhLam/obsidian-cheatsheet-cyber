# [WPS Reconnaissance](WPS%20Reconnaissance.md)
### How many WIFI networks with WPS are available? (Answer in digit format: e.g., 5)

```sh
sudo -s

airmon-ng start wlan0

airdump-ng --wps --ignore-negative-one wlan0mon
```

![](Screenshot%202026-09-21%20at%2008.10.40.png)
### What is the name of the WIFI network with the BSSID D8:D7:3D:EB:29:D5?

CyberNetSecure