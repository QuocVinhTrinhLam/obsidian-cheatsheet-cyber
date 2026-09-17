### Aircrack-ng Benchmark

```sh
3kjS@htb[/htb]$ aircrack-ng -S

1628.101 k/s
```
### Cracking WEP

```sh
3kjS@htb[/htb]$ aircrack-ng -K HTB.ivs
```
### Cracking WPA

```sh
3kjS@htb[/htb]$ aircrack-ng HTB.pcap -w /opt/wordlist.txt
```
