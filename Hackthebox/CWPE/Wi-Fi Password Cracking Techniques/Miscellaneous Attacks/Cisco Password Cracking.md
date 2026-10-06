|**Cisco Password type**|**Ability to crack**|**Vulnerability severity**|**Recommendation**|
|---|---|---|---|
|`Type 0`|Immediate|Critical|Do not use|
|`Type 4`|Easy|Critical|Do not use|
|`Type 5`|Medium|Medium|Use only when Types 6, 8, and 9 are not available|
|`Type 6`|Difficult|Low|Use only when reversible encryption is needed, or when Type 8 is not available|
|`Type 7`|Immediate|Critical|Do not use|
|`Type 8`|Difficult|Low|Recommended|
|`Type 9`|Difficult|Low|Recommended|
### Cisco Type 0 Passwords

```config
username tom password 0 P@ssw0rd
```
### Cisco Type 4 Passwords

```config
username bob secret 4 g1rTD89b38NIXbGJse.zLc7Cega1TBTlKQNvYDh9Qo6
```

```hash
g1rTD89b38NIXbGJse.zLc7Cega1TBTlKQNvYDh9Qo6
```

```sh
3kjS@htb[/htb]$ john --format=Raw-SHA256 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

```sh
3kjS@htb[/htb]$ hashcat -m 5700 -a 0 hash /usr/share/wordlists/rockyou.txt
```

- `-m 5700`: Specifies the SHA-256 hash mode.
- `-O`: Enables optimized kernels for faster cracking, though it limits password length to 31 characters.
- `-a 0`: Selects a straight dictionary attack mode.
- `/usr/share/wordlists/rockyou.txt`: The path to the wordlist.
### Cisco Type 5 Passwords

```config
username bob secret 5 $1$w1Jm$bCt7eJNv.CjWPwyfWcobP0
```

```sh
3kjS@htb[/htb]$ john --format=md5crypt --fork=4 --wordlist=/usr/share/wordlists/rockyou.txt hash
```

```sh
3kjS@htb[/htb]$ hashcat -m 500 -a 0 hash /usr/share/wordlists/rockyou.txt
```
### Cisco Type 6 Passwords

```console
configure terminal
(config)# password encryption aes
(config)# key config-key password-encrypt MyS3cretkey
(config)# username sentinal password ciscoadmin123
(config)# username mrgrep password mrgrep555
end
```

```config
username sentinal password 6 fZbe^WdXO`^O[YF`XLCfBV\BK`hMge]HF
username mrgrep password 6 AXSQ]]_MZWLPdhBSP[FAc]E`\`EH`]AGK
```
### Cisco Type 7 Passwords

```config
username bob password 7 08116C5D1A0E550516
```

```sh
3kjS@htb[/htb]$ wget https://raw.githubusercontent.com/theevilbit/ciscot7/master/ciscot7.py
3kjS@htb[/htb]$ python ciscot7.py -d -p 08116C5D1A0E550516
```

- [Firewall.cx](https://www.firewall.cx/cisco/cisco-routers/cisco-type7-password-crack.html): Cisco Type 7 Password Decrypt
- [IFM Password Cracker](https://www.ifm.net.nz/cookbooks/passwordcracker.html): Cisco Type 7 Password Decrypt
### Cisco Type 8 Passwords

```config
username tom secret 8 $8$kMehFGHe4ew.chRm.d3hge68ECor21viE35NAMV72qPho75fl/lsFlyEFl
```

```hash
$8$kMehFGHe4ew.chRm.d3hge68ECor21viE35NAMV72qPho75fl/lsFlyEFl
```

```sh
3kjS@htb[/htb]$ john --format=pbkdf2-hmac-sha256 --fork=4 --wordlist=/usr/share/wordlists/rockyou.txt hash
```

```sh
3kjS@htb[/htb]$ hashcat -m 9200 -a 0 hash /usr/share/wordlists/rockyou.txt
```
### Cisco Type 9 Passwords

```config
username tom secret 9 $9$ApsgnGtdkTswkfjucj./4w7dcjhGFsjkdT7mAup2lveHuu25fL.hgvfiq
```

```hash
$9$ApsgnGtdkTswkfjucj./4w7dcjhGFsjkdT7mAup2lveHuu25fL.hgvfiq
```

```sh
3kjS@htb[/htb]$ john --format=scrypt --fork=4 --wordlist=/usr/share/wordlists/rockyou.txt hash
```

```sh
3kjS@htb[/htb]$ hashcat -m 9300 -a 0 --force hash /usr/share/wordlists/rockyou.txt
```
## Security Recommendations:

- `Avoid weak types`: Type 0, 4, 5, and 7 should be avoided.
- `Consider Type 6 for specific needs`: If reversible encryption is required, Type 6 can be used.
- `Prioritize Type 8 and Type 9`: These are the strongest and most recommended options.

The NSA has also published a document on best practices for managing Cisco password types. It outlines how each password type works, highlights their weaknesses, and offers clear recommendations for choosing stronger encryption methods and avoiding insecure configurations: [Cisco Password Types Best Practices](https://media.defense.gov/2022/Feb/17/2002940795/-1/-1/1/CSI_CISCO_PASSWORD_TYPES_BEST_PRACTICES_20220217.PDF).