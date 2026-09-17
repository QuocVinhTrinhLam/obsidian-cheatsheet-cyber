# [Aircrack-ng](Aircrack-ng.md)
### Utilize Aircrack-ng to crack the WEP key from the file located at "/opt/WEP.ivs" and submit the found key as the answer.

```sh
sudo aircrack-ng -K WEP.ivs
```

![](Screenshot%202026-09-16%20at%2015.04.27.png)
### Utilize Aircrack-ng to crack the WPA key for the ESSID "Coherer" from the file located at "/opt/WPA_Capture.pcap" and submit the found key as the answer.

```sh
sudo aircrack-ng WPA_Capture.pcap -w wordlists.txt
```

![](Screenshot%202026-09-16%20at%2015.07.43.png)