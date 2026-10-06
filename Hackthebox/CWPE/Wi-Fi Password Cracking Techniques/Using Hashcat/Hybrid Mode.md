### Mode 6: Dictionary Followed by Mask

```sh
3kjS@htb[/htb]$ hashcat -a 6 -m 22000 wpa_hash wordlist.txt ?d?d?d
```
### Mode 7: Mask Followed by Dictionary

```sh
3kjS@htb[/htb]$ hashcat -a 7 -m 22000 wpa_hash ?d?d?d wordlist.txt
```
