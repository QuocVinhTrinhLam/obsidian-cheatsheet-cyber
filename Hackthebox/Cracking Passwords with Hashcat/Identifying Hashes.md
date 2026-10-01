```sh
$1$  : MD5
$2a$ : Blowfish
$2y$ : Blowfish, with correct handling of 8 bit characters
$5$  : SHA256
$6$  : SHA512
```
## Hashid

```sh
3kjS@htb[/htb]$ pip install hashid
```

Hashes can be supplied as command-line arguments or using a file.

```sh
3kjS@htb[/htb]$ hashid '$apr1$71850310$gh9m4xcAn3MGxogwX/ztb.'

Analyzing '$apr1$71850310$gh9m4xcAn3MGxogwX/ztb.'
[+] MD5(APR) 
[+] Apache MD5
```

```sh
3kjS@htb[/htb]$ hashid hashes.txt 

--File 'hashes.txt'--
Analyzing '2fc5a684737ce1bf7b3b239df432416e0dd07357:2014'
[+] SHA-1 
[+] Double SHA-1 
[+] RIPEMD-160 
[+] Haval-160 
[+] Tiger-160 
[+] HAS-160 
[+] LinkedIn 
[+] Skein-256(160) 
[+] Skein-512(160) 
[+] Redmine Project Management Web App 
[+] SMF ≥ v1.1 
Analyzing '$P$984478476IagS59wHZvyQMArzfx58u.'
[+] Wordpress ≥ v2.6.2 
[+] Joomla ≥ v2.5.18 
[+] PHPass' Portable Hash 
--End of file 'hashes.txt'--
```

```sh
3kjS@htb[/htb]$ hashid '$DCC2$10240#tom#e4e938d12fe5974dc42a90120bd9c90f' -m Analyzing '$DCC2$10240#tom#e4e938d12fe5974dc42a90120bd9c90f' 
[+] Domain Cached Credentials 2 [Hashcat Mode: 2100
```
## Context is Important

For example, passing the hash `a2d1f7b7a1862d0d4a52644e72d59df5:500:lp@trash-mail.com` to `hashid` will give us various possibilities:

```
3kjS@htb[/htb]$ hashid 'a2d1f7b7a1862d0d4a52644e72d59df5:500:lp@trash-mail.com'

Analyzing 'a2d1f7b7a1862d0d4a52644e72d59df5:500:lp@trash-mail.com'
[+] MD5 
[+] MD4 
[+] Double MD5 
[+] LM 
[+] RIPEMD-128 
[+] Haval-128 
[+] Tiger-128 
[+] Skein-256(128) 
[+] Skein-512(128) 
[+] Lotus Notes/Domino 5 
[+] Skype 
[+] Lastpass 
```
