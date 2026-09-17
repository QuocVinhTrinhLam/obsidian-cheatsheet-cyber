## Using GUI

Here’s a breakdown of how this process usually works using GUI:

1. `Scan for Networks`
2. `Select the Network`
3. `Enter Credentials`
4. `Connect`

![](Connecting%20to%20Wi-Fi%20Networks-20260916-150914.png)

![](Connecting%20to%20Wi-Fi%20Networks-20260916-150919.png)

![](Connecting%20to%20Wi-Fi%20Networks-20260916-150922.png)
## Using CLI

```sh
3kjS@htb[/htb]$ sudo iwlist wlan0 s | grep 'Cell\|Quality\|ESSID\|IEEE'

          Cell 01 - Address: D8:D6:3D:EB:29:D5
                    Quality=61/70  Signal level=-49 dBm  
                    ESSID:"HackMe"
                    IE: IEEE 802.11i/WPA2 Version 1
          Cell 02 - Address: 3E:C1:D0:F2:5D:6A
                    Quality=70/70  Signal level=-30 dBm  
                    ESSID:"HackTheBox"
          Cell 03 - Address: 9C:9A:03:39:BD:71
                    Quality=70/70  Signal level=-30 dBm  
                    ESSID:"HTB-Corp"
                    IE: IEEE 802.11i/WPA2 Version 1
```
#### Connecting to WEP Networks

```config
network={
    ssid="HackTheBox"
    key_mgmt=NONE
    wep_key0=3C1C3A3BAB
    wep_tx_keyidx=0
}
```

```sh
3kjS@htb[/htb]$ sudo wpa_supplicant -c wep.conf -i wlan0
```

After connecting, we can obtain an IP address by using the `dhclient` utility. This will assign an IP from the network's DHCP server, completing the connection setup.

```sh
3kjS@htb[/htb]$ sudo dhclient wlan0
```

```sh
3kjS@htb[/htb]$ ifconfig wlan0

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.2.7  netmask 255.255.255.0  broadcast 192.168.2.255
        ether f6:65:bc:77:c9:21  txqueuelen 1000  (Ethernet)
        RX packets 7  bytes 1217 (1.2 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 14  bytes 3186 (3.1 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
#### Connecting to WPA Personal Networks

```config
network={
    ssid="HackMe"
    psk="password123"
}
```

Then we could initiate our wpa connection to the AP using the following command.

```sh
3kjS@htb[/htb]$ sudo wpa_supplicant -c wpa.conf -i wlan0
```

After connecting, we can obtain an IP address by using the `dhclient` utility. This will assign an IP from the network's DHCP server, completing the connection setup. However, if we have a previously assigned DHCP IP address from a different connection, we'll need to release it first. Run the following command to remove the existing IP address:

```
3kjS@htb[/htb]$ sudo dhclient wlan0 -r

Killed old client process
```

We can now run the dhclient command. This will assign an IP from the network's DHCP server, completing the connection setup.

```sh
3kjS@htb[/htb]$ sudo dhclient wlan0
```

```sh
3kjS@htb[/htb]$ ifconfig wlan0

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.7  netmask 255.255.255.0  broadcast 192.168.1.255
        ether f6:65:bc:77:c9:21  txqueuelen 1000  (Ethernet)
        RX packets 37  bytes 6266 (6.2 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 41  bytes 6967 (6.9 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
#### Connecting to WPA Enterprise

```config
network={
  ssid="HTB-Corp"
  key_mgmt=WPA-EAP
  identity="HTB\Administrator"
  password="Admin@123"
}
```

```sh
3kjS@htb[/htb]$ sudo wpa_supplicant -c wpa_enterprise.conf -i wlan0
```

```sh
3kjS@htb[/htb]$ sudo dhclient wlan0 -r

Killed old client process
```

```sh
3kjS@htb[/htb]$ sudo dhclient wlan0
```

```sh
3kjS@htb[/htb]$ ifconfig wlan0

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.3.7  netmask 255.255.255.0  broadcast 192.168.3.255
        ether f6:65:bc:77:c9:21  txqueuelen 1000  (Ethernet)
        RX packets 66  bytes 10226 (10.2 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 77  bytes 11532 (11.5 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
#### Connecting with Network Manager Utilities

```sh
3kjS@htb[/htb]$ sudo nmtui
```

![](Connecting%20to%20Wi-Fi%20Networks-20260916-151534.png)

![](Connecting%20to%20Wi-Fi%20Networks-20260916-151539.png)

