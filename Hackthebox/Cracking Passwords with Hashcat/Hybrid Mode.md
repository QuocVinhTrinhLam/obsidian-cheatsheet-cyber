#### Creating Hybrid Hash

```sh
3kjS@htb[/htb]$ echo -n 'football1$' | md5sum | tr -d " -" > hybrid_hash
```
#### Hashcat - Hybrid Attack using Wordlists

```sh
3kjS@htb[/htb]$ hashcat -a 6 -m 0 hybrid_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt '?d?s'
```
#### Creating another Hybrid Hash

```sh
3kjS@htb[/htb]$ echo -n '2015football' | md5sum | tr -d " -" > hybrid_hash_prefix 
```
#### Hashcat - Hybrid Attack using Wordlists with Masks

```sh
3kjS@htb[/htb]$ hashcat -a 7 -m 0 hybrid_hash_prefix -1 01 '20?1?d' /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```
