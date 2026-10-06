To demonstrate this attack, consider the following wordlists:

```sh
3kjS@htb[/htb]$ cat wordlist1

super
world
secret
```

```sh
3kjS@htb[/htb]$ cat wordlist2

hello
password
```

```sh
3kjS@htb[/htb]$ awk '(NR==FNR) { a[NR]=$0 } (NR != FNR) { for (i in a) { print $0 a[i] } }' file2 file1 superhello superpassword worldhello wordpassword secrethello secretpassword
```
## Performing the Attack

```sh
3kjS@htb[/htb]$ hashcat -m 22000 -a 1 wpa_hash wordlist1 wordlist2
```
