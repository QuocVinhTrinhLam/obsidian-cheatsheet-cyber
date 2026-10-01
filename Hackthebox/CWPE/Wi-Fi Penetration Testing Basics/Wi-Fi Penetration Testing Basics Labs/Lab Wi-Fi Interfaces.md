# [Wi-Fi Interfaces](Wi-Fi%20Interfaces.md)
### Check the driver capabilities for the interface. How many software interface modes are available? (Answer in digit format: e.g., 3)

```sh
iw list | grep -A 10 software
```
![](Screenshot%202026-09-16%20at%2010.56.37.png)
### Follow the steps shown in the section to scan for available WiFi networks. What is the ESSID name of the 3rd WiFi Network (Cell 03)?

```sh
iwlist wlan0 scan | grep 'Cell\|Quality\|ESSID\|IEEE'
```
![](Screenshot%202026-09-16%20at%2010.59.55.png)

