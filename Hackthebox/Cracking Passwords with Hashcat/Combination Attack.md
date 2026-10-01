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

If given these two word lists `Hashcat` will produce exactly 3 x 2 = 6 words, such as the following:

```sh
3kjS@htb[/htb]$ awk '(NR==FNR) { a[NR]=$0 } (NR != FNR) { for (i in a) { print $0 a[i] } }' file2 file1

superhello
superpassword
worldhello
wordpassword
secrethello
secretpassword
```

```sh
3kjS@htb[/htb]$ hashcat -a 1 --stdout file1 file2 
superhello 
superpassword 
worldhello 
worldpassword 
secrethello 
secretpassword
```
#### Hashcat - Syntax

```sh
3kjS@htb[/htb]$ hashcat -a 1 -m <hash type> <hash file> <wordlist1> <wordlist2>
```

```sh
3kjS@htb[/htb]$ echo -n 'secretpassword' | md5sum | cut -f1 -d' '  > combination_md5

2034f6e32958647fdff75d265b455ebf
```

```sh
3kjS@htb[/htb]$ hashcat -a 1 -m 0 combination_md5 wordlist1 wordlist2
```
