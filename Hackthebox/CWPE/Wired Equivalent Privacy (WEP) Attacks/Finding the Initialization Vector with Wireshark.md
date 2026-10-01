Let's begin by listing the available wireless interfaces on our attack host.

```sh
3kjS@htb[/htb]$ iwconfig

lo        no wireless extensions.

eth0      no wireless extensions.

wlan0     IEEE 802.11  ESSID:off/any  
         Mode:Managed  Access Point: Not-Associated   Tx-Power=20 dBm   
         Retry short  long limit:2   RTS thr:off   Fragment thr:off
         Power Management:off
```

Prior to scanning, we must enable monitor mode.

```sh
3kjS@htb[/htb]$ sudo airmon-ng start wlan0

Found 4 processes that could cause trouble.
Kill them using 'airmon-ng check kill' before putting
the card in monitor mode, they will interfere by changing channels
and sometimes putting the interface back in managed mode

   PID Name
   602 avahi-daemon
   614 avahi-daemon
   700 NetworkManager
   701 wpa_supplicant

PHY     Interface       Driver          Chipset

phy0    wlan0           rt2800usb       Ralink Technology, Corp. RT****
```

Should there be any conflicting processes, the following command will kill them.

```sh
3kjS@htb[/htb]$ sudo airmon-ng check kill

Killing these processes:

   PID Name
   701 wpa_supplicant
```

We can now resume our efforts, broadly scanning in search of wireless networks that use WEP.

```sh
3kjS@htb[/htb]$ sudo airodump-ng wlan0mon
```

```sh
3kjS@htb[/htb]$ sudo airodump-ng -c 3 --bssid 60:38:E0:71:E9:DC wlan0mon -w WEP
```

After scanning for a few seconds, we can terminate the session and open the capture file in Wireshark. By selecting any IEEE [802.11 data packet](https://wiki.wireshark.org/Wi-Fi) and expanding the `'IEEE 802.11 Data'` and `'WEP Parameters'` sections, we can view the packet's initialization vector (IV) along with the message ICV (CRC32).

![](Finding%20the%20Initialization%20Vector%20with%20Wireshark-20260921-142659.png)
