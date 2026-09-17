# [Aireplay-ng](Aireplay-ng.md)
### Set the channel to 11 and test for packet injection using aireplay-ng. On how many APs does it perform packet injection? (Answer in digit format: e.g., 3)

```sh
sudo iw dev wlan0mon set channel 11

sudo aireplay-ng --test wlan0mon
```
![](Screenshot%202026-09-16%20at%2014.31.16.png)
### How many clients are connected to 'CyberNet-Secure'? (Answer in digit format: e.g., 3)

![](Screenshot%202026-09-16%20at%2014.39.25.png)