#### Managed Mode

```sh
3kjS@htb[/htb]$ sudo ifconfig wlan0 down
3kjS@htb[/htb]$ sudo iwconfig wlan0 mode managed
```

Then, to connect to a network, we could utilize the following command.

```sh
3kjS@htb[/htb]$ sudo iwconfig wlan0 essid HTB-Wifi
```

Then, to check our interface, we can utilize the `iwconfig` utility.

```sh
3kjS@htb[/htb]$ sudo iwconfig

wlan0     IEEE 802.11  ESSID:"HTB-Wifi"  
          Mode:Managed  Access Point: Not-Associated   Tx-Power=30 dBm   
          Retry short  long limit:2   RTS thr:off   Fragment thr:off
          Power Management:off
```
#### Ad-hoc Mode

To set our interface into this mode, we would run the following commands.

```sh
3kjS@htb[/htb]$ sudo iwconfig wlan0 mode ad-hoc
3kjS@htb[/htb]$ sudo iwconfig wlan0 essid HTB-Mesh
```

Then, once again, we could check our interface with the `iwconfig` command.

```
3kjS@htb[/htb]$ sudo iwconfig

wlan0     IEEE 802.11  ESSID:"HTB-Mesh"  
          Mode:Ad-Hoc  Frequency:2.412 GHz  Cell: Not-Associated   
          Tx-Power=30 dBm   
          Retry short  long limit:2   RTS thr:off   Fragment thr:off
          Power Management:off
```
#### Master Mode

```sh
3kjS@htb[/htb]$ nano open.conf

interface=wlan0
driver=nl80211
ssid=HTB-Hello-World
channel=2
hw_mode=g
```

This configuration would simply bring up an open network with the name HTB-Hello-World. With this network configuration, we could bring it up with the following command.

```sh
3kjS@htb[/htb]$ sudo hostapd open.conf

wlan0: interface state UNINITIALIZED->ENABLED
wlan0: AP-ENABLED 
wlan0: STA 2c:6d:c1:af:eb:91 IEEE 802.11: authenticated
wlan0: STA 2c:6d:c1:af:eb:91 IEEE 802.11: associated (aid 1)
wlan0: AP-STA-CONNECTED 2c:6d:c1:af:eb:91
wlan0: STA 2c:6d:c1:af:eb:91 RADIUS: starting accounting session D249D3336F052567
```
#### Mesh Mode

```sh
3kjS@htb[/htb]$ sudo iw dev wlan0 set type mesh
```

Then we can check our interface once again with the `iwconfig` utility.

```sh
3kjS@htb[/htb]$ sudo iwconfig

wlan0     IEEE 802.11  Mode:Auto  Tx-Power=30 dBm   
          Retry short  long limit:2   RTS thr:off   Fragment thr:off
          Power Management:off
```
#### Monitor Mode

First we would need to bring our interface down to avoid a device or resource busy error.

```sh
3kjS@htb[/htb]$ sudo ifconfig wlan0 down
```

Then we could set our interface's mode with iw {interface name} set {mode}

```sh
3kjS@htb[/htb]$ sudo iw wlan0 set monitor control
```

Then we can bring our interface back up.

```sh
3kjS@htb[/htb]$ sudo ifconfig wlan0 up
```

Finally, to ensure that our interface is in monitor mode, we can utilize the `iwconfig` utility.

```sh
3kjS@htb[/htb]$ iwconfig

wlan0     IEEE 802.11  Mode:Monitor  Frequency:2.457 GHz  Tx-Power=30 dBm   
          Retry short  long limit:2   RTS thr:off   Fragment thr:off
          Power Management:off
```

Overall, it is important to make sure our interface supports whatever mode is pertinent to our testing efforts. If we are attempting to exploit WEP, WPA, WPA2, WPA3, and all enterprise variants, we are likely sufficient with just monitor mode and packet injection capabilities. However, suppose we were trying to achieve different actions we might consider the following capabilities.

1. `Employing a Rogue AP or Evil-Twin Attack:` - We would want our interface to support master mode with a management daemon like hostapd, hostapd-mana, hostapd-wpe, airbase-ng, and others.
2. `Backhaul and Mesh or Mesh-Type system exploitation:` - We would want to make sure our interface supports ad-hoc and mesh modes accordingly. For this kind of exploitation we are normally sufficient with monitor mode and packet injection, but the extra capabilities can allow us to perform node impersonation among others.
