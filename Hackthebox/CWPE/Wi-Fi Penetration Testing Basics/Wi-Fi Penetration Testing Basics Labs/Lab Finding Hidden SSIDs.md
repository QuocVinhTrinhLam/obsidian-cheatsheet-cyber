# [Finding Hidden SSIDs](Finding%20Hidden%20SSIDs.md)
### Identify the name of the hidden SSID with the BSSID d8:d6:3d:eb:29:d5 and submit it as your answer.

First set interface 'wlan0' to monitor mode 

```sh
sudo airmon-ng start wlan0
```

![](Screenshot%202026-09-16%20at%2016.02.29.png)

```sh
sudo airodump-ng -c wlan0mon
```

![](Screenshot%202026-09-16%20at%2016.04.02.png)
### Identify the name of the hidden SSID with the BSSID a2:a6:32:1b:29:d5 and submit it as your answer.

```sh
sudo mdk3 wlan0mon p -b u -c 1 -t a2:a6:32:1b:29:d5
```
![](Screenshot%202026-09-16%20at%2016.16.02.png)
### Identify the name of the hidden SSID with the BSSID d2:a3:32:1b:29:d5 and submit it as your answer.

```sh
sudo mdk3 wlan0mon p -f /opt/wordlist.txt -t d2:a3:32:1b:29:d5
```
![](Screenshot%202026-09-16%20at%2016.18.27.png)