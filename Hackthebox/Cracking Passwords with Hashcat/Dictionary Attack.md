## Straight or Dictionary Attack
#### Hashcat - Syntax

```sh
3kjS@htb[/htb]$ hashcat -a 0 -m <hash type> <hash file> <wordlist>
```

```sh
3kjS@htb[/htb]$ echo -n '!academy' | sha256sum | cut -f1 -d' ' > sha256_hash_example 3kjS@htb[/htb]$ hashcat -a 0 -m 1400 sha256_hash_example /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```
