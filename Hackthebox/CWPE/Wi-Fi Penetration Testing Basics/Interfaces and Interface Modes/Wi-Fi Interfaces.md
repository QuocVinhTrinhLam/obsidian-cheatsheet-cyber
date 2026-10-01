#### Interface Strength

```sh
3kjS@htb[/htb]$ iwconfig

wlan0     IEEE 802.11  ESSID:off/any  
          Mode:Managed  Access Point: Not-Associated   Tx-Power=20 dBm   
          Retry short  long limit:2   RTS thr:off   Fragment thr:off
          Power Management:off
```

```sh
3kjS@htb[/htb]$ iw reg get
```
#### Changing the Region Settings for our Interface

```sh
3kjS@htb[/htb]$ sudo iw reg set US
```

Then, we could check this setting again with the iw reg get command.

```sh
3kjS@htb[/htb]$ iw reg get

global
country US: DFS-FCC
        (902 - 904 @ 2), (N/A, 30), (N/A)
        (904 - 920 @ 16), (N/A, 30), (N/A)
        (920 - 928 @ 8), (N/A, 30), (N/A)
        (2400 - 2472 @ 40), (N/A, 30), (N/A)
        (5150 - 5250 @ 80), (N/A, 23), (N/A), AUTO-BW
        (5250 - 5350 @ 80), (N/A, 24), (0 ms), DFS, AUTO-BW
        (5470 - 5730 @ 160), (N/A, 24), (0 ms), DFS
        (5730 - 5850 @ 80), (N/A, 30), (N/A), AUTO-BW
        (5850 - 5895 @ 40), (N/A, 27), (N/A), NO-OUTDOOR, AUTO-BW, PASSIVE-SCAN
        (5925 - 7125 @ 320), (N/A, 12), (N/A), NO-OUTDOOR, PASSIVE-SCAN
        (57240 - 71000 @ 2160), (N/A, 40), (N/A)
```

Afterwards, we can check the txpower of our interface with the `iwconfig` utility.

```sh
3kjS@htb[/htb]$ iwconfig

wlan0     IEEE 802.11  ESSID:off/any  
          Mode:Managed  Access Point: Not-Associated   Tx-Power=20 dBm   
          Retry short  long limit:2   RTS thr:off   Fragment thr:off
          Power Management:off
```

In many cases, our interface will automatically set its power to the maximum in our region. However, sometimes we might need to do this ourselves. First, we would have to bring our interface down.

```sh
3kjS@htb[/htb]$ sudo ifconfig wlan0 down
```

Then, we can set the desired txpower for our interface with the `iwconfig` utility.

```sh
3kjS@htb[/htb]$ sudo iwconfig wlan0 txpower 30
```

After that, we would need to bring our interface back up.

```sh
3kjS@htb[/htb]$ sudo ifconfig wlan0 up
```

```sh
3kjS@htb[/htb]$ iwconfig
```
#### Checking Driver Capabilities for our Interface

The command that we can use to find out this information is the iw list command.

```sh
3kjS@htb[/htb]$ iw list
```

1. `Almost all pertinent regular ciphers`
2. `Both 2.4Ghz and 5Ghz bands`
3. `Mesh networks and IBSS capabilities`
4. `P2P peering`
5. `SAE aka WPA3 authentication`
#### Scanning Available WiFi Networks

```sh
3kjS@htb[/htb]$ iwlist wlan0 scan |  grep 'Cell\|Quality\|ESSID\|IEEE'

          Cell 01 - Address: f0:28:c8:d9:9c:6e
                    Quality=61/70  Signal level=-49 dBm  
                    ESSID:"HTB-Wireless"
                    IE: IEEE 802.11i/WPA2 Version 1
          Cell 02 - Address: 3a:c4:6e:40:09:76
                    Quality=70/70  Signal level=-30 dBm  
                    ESSID:"CyberCorp"
                    IE: IEEE 802.11i/WPA2 Version 1
          Cell 03 - Address: 48:32:c7:a0:aa:6d
                    Quality=70/70  Signal level=-30 dBm  
                    ESSID:"HackTheBox"
                    IE: IEEE 802.11i/WPA2 Version 1
```
#### Changing Channel & Frequency of Interface

```sh
3kjS@htb[/htb]$ iwlist wlan0 channel

wlan0     32 channels in total; available frequencies :
          Channel 01 : 2.412 GHz
          Channel 02 : 2.417 GHz
          Channel 03 : 2.422 GHz
          Channel 04 : 2.427 GHz
          <SNIP>
          Channel 140 : 5.7 GHz
          Channel 149 : 5.745 GHz
          Channel 153 : 5.765 GHz
```

```sh
3kjS@htb[/htb]$ sudo ifconfig wlan0 down 
3kjS@htb[/htb]$ sudo iwconfig wlan0 channel 64 
3kjS@htb[/htb]$ sudo ifconfig wlan0 up 
3kjS@htb[/htb]$ iwlist wlan0 channel
```

First, we need to disable the wireless interface which ensures that the interface is not in use and can be safely reconfigured. Then we can set the desired `channel` using the `iwconfig` command and finally, re-enable the wireless interface.

If we prefer to change the frequency directly rather than adjusting the channel, we have the option to do so as well.

```sh
3kjS@htb[/htb]$ iwlist wlan0 frequency | grep Current

          Current Frequency:5.32 GHz (Channel 64)
```

To change the frequency, we first need to disable the wireless interface, which ensures that the interface is not in use and can be safely reconfigured. Then, we can set the desired frequency using the iwconfig command and finally, re-enable the wireless interface.

```sh
3kjS@htb[/htb]$ sudo ifconfig wlan0 down
3kjS@htb[/htb]$ sudo iwconfig wlan0 freq "5.52G"
3kjS@htb[/htb]$ sudo ifconfig wlan0 up
```

```sh
3kjS@htb[/htb]$ iwlist wlan0 frequency | grep Current

          Current Frequency:5.52 GHz (Channel 104)
```
