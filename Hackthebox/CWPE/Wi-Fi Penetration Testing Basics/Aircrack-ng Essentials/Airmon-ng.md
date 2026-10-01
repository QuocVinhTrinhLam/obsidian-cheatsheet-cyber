### Starting monitor mode

```sh
3kjS@htb[/htb]$ sudo airmon-ng

PHY     Interface       Driver          Chipset

phy0    wlan0           rt2800usb       Ralink Technology, Corp. RT2870/RT3070
```

We can set the wlan0 interface into monitor mode using the command `airmon-ng start wlan0`.

```sh
3kjS@htb[/htb]$ sudo airmon-ng start wlan0
```

We could test to see if our interface is in monitor mode with the iwconfig utility.

```sh
3kjS@htb[/htb]$ iwconfig

wlan0mon  IEEE 802.11  Mode:Monitor  Frequency:2.457 GHz  Tx-Power=30 dBm   
          Retry short  long limit:2   RTS thr:off   Fragment thr:off
          Power Management:off
```
### Checking for interfering processes

```sh
3kjS@htb[/htb]$ sudo airmon-ng check

Found 5 processes that could cause trouble.
If airodump-ng, aireplay-ng or airtun-ng stops working after
a short period of time, you may want to kill (some of) them!

  PID Name
  718 NetworkManager
  870 dhclient
 1104 avahi-daemon
 1105 avahi-daemon
 1115 wpa_supplicant
```

```sh
3kjS@htb[/htb]$ sudo airmon-ng check kill

Killing these processes:

  PID Name
  870 dhclient
 1115 wpa_supplicant
```
### Starting monitor mode on a specific channel

```sh
3kjS@htb[/htb]$ sudo airmon-ng start wlan0 11
```
![](Screenshot%202026-09-16%20at%2011.32.21.png)
### Stopping monitor mode

```sh
3kjS@htb[/htb]$ sudo airmon-ng stop wlan0mon

PHY     Interface       Driver          Chipset

phy0    wlan0mon        rt2800usb       Ralink Technology, Corp. RT2870/RT3070
                (mac80211 station mode vif enabled on [phy0]wlan0)
                (mac80211 monitor mode vif disabled for [phy0]wlan0)
```

We could test to see if our interface is back to managed mode with the iwconfig utility.

```sh
3kjS@htb[/htb]$ iwconfig

wlan0  IEEE 802.11  Mode:Managed  Frequency:2.457 GHz  Tx-Power=30 dBm   
          Retry short  long limit:2   RTS thr:off   Fragment thr:off
          Power Management:off
```
