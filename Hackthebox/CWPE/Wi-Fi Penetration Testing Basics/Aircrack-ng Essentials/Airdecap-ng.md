### Using Airdecap-ng

```sh
airdecap-ng [options] <pcap file>
```

|**Option**|**Description**|
|---|---|
|`-l`|don't remove the 802.11 header|
|`-b`|access point MAC address filter|
|`-k`|WPA/WPA2 Pairwise Master Key in hex|
|`-e`|target network ascii identifier|
|`-p`|target network WPA/WPA2 passphrase|
|`-w`|target network WEP key in hexadecimal|
![](Airdecap-ng-20260916-144013.png)

![](Airdecap-ng-20260916-144121.png)
### Removing Wireless Headers from Unencrypted Capture file

```usage
airdecap-ng -b <bssid> <capture-file>
```

```sh
3kjS@htb[/htb]$ sudo airdecap-ng -b 00:14:6C:7A:41:81 opencapture.cap

Total number of stations seen            0
Total number of packets read           251
Total number of WEP data packets         0
Total number of WPA data packets         0
Number of plaintext data packets         0
Number of decrypted WEP  packets         0
Number of corrupted WEP  packets         0
Number of decrypted WPA  packets         0
Number of bad TKIP (WPA) packets         0
Number of bad CCMP (WPA) packets         0
```
### Decrypting WEP-encrypted captures

To decrypt a WEP-encrypted capture file using Airdecap-ng, we can use the following command:

```usage
airdecap-ng -w <WEP-key> <capture-file>
```

Replace with the hexadecimal WEP key and with the name of the capture file.

For example:

```sh
3kjS@htb[/htb]$ sudo airdecap-ng -w 1234567890ABCDEF HTB-01.cap

Total number of stations seen            6
Total number of packets read           356
Total number of WEP data packets       235
Total number of WPA data packets       121
Number of plaintext data packets         0
Number of decrypted WEP  packets         0
Number of corrupted WEP  packets         0
Number of decrypted WPA  packets       235
Number of bad TKIP (WPA) packets         0
Number of bad CCMP (WPA) packets         0
```
### Decrypting WPA-encrypted captures

To decrypt a WPA-encrypted capture file using Airdecap-ng, we can use the following command:

```sh
airdecap-ng -p <passphrase> <capture-file> -e <essid>
```

Replace with the WPA passphrase, with the name of the capture file and with the ESSID name of the respective network.

For example:

```sh
3kjS@htb[/htb]$ sudo airdecap-ng -p 'abdefg' HTB-01.cap -e "Wireless Lab"

Total number of stations seen            6
Total number of packets read           356
Total number of WEP data packets       235
Total number of WPA data packets       121
Number of plaintext data packets         0
Number of decrypted WEP  packets         0
Number of corrupted WEP  packets         0
Number of decrypted WPA  packets       121
Number of bad TKIP (WPA) packets         0
Number of bad CCMP (WPA) packets         0
```
