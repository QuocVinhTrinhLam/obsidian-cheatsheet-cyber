| **Field** | **Description**                                                                          |
| --------- | ---------------------------------------------------------------------------------------- |
| `BSSID`   | Shows the MAC address of the access points                                               |
| `PWR`     | Shows the "power" of the network. The higher the number, the better the signal strength. |
| `Beacons` | Shows the number of announcement packets sent by the network.                            |
| `#Data`   | Shows the number of captured data packets.                                               |
| `#/s`     | Shows the number of data packets captured in the past ten seconds.                       |
| `CH`      | Shows the "Channel" the network runs on.                                                 |
| `MB`      | Shows the maximum speed supported by the network.                                        |
| `ENC`     | Shows the encryption method used by the network.                                         |
| `CIPHER`  | Shows the cipher used by the network.                                                    |
| `AUTH`    | Shows the authentication used by the network.                                            |
| `ESSID`   | Shows the name of the network.                                                           |
| `STATION` | Shows the MAC address of the client connected to the network.                            |
| `RATE`    | Shows the data transfer rate between the client and the access point.                    |
| `LOST`    | Shows the number of data packets lost.                                                   |
| `Packets` | Shows the number of data packets sent by the client.                                     |
| `Notes`   | Shows additional information about the client, such as captured EAPOL or PMKID.          |
| `PROBES`  | Shows the list of networks the client is probing for.                                    |

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

eth0      no wireless extensions.

wlan0mon  IEEE 802.11  Mode:Monitor  Frequency:2.457 GHz  Tx-Power=20 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:on
          
lo        no wireless extensions.
```

```sh
3kjS@htb[/htb]$ sudo airodump-ng wlan0mon
```
![](Screenshot%202026-09-16%20at%2013.48.34.png)
### Scanning Specific Channels or a Single Channel

Example of a single channel:

```sh
3kjS@htb[/htb]$ sudo airodump-ng -c 11 wlan0mon
```
![](Screenshot%202026-09-16%20at%2013.52.24.png)

It is also possible to select multiple channels for scanning using the command `airodump-ng -c 1,6,11 wlan0mon`.
### Scanning 5 GHz Wi-Fi bands

The supported bands are a, b, and g.

- `a` uses 5 GHz
- `b` uses 2.4 GHz
- `g` uses 2.4 GHz

```sh
3kjS@htb[/htb]$ sudo airodump-ng wlan0mon --band a
```
![](Screenshot%202026-09-16%20at%2013.54.45.png)
### Saving the output to a file

```sh
3kjS@htb[/htb]$ sudo airodump-ng wlan0mon -w HTB
```
![](Screenshot%202026-09-16%20at%2013.55.42.png)

```sh
3kjS@htb[/htb]$ ls

HTB-01.csv   HTB-01.kismet.netxml   HTB-01.cap   HTB-01.kismet.csv   HTB-01.log.csv 
```
