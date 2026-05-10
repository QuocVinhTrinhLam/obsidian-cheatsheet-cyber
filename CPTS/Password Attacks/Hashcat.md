
```shell
3kjS@htb[/htb]$ hashcat -a 0 -m 0 <hashes> [wordlist, rule, mask, ...]
```

In the command above:

- `-a` is used to specify the `attack mode`
- `-m` is used to specify the `hash type`
- `<hashes>` is a either a hash string, or a file containing one or more password hashes of the same type
- `[wordlist, rule, mask, ...]` is a placeholder for additional arguments that depend on the attack mode
## Attack modes
#### Dictionary attack

```shell
3kjS@htb[/htb]$ hashcat -a 0 -m 0 e3e3ec5831ad5e7288241960e5d4fdb8 /usr/share/wordlists/rockyou.txt
```

```shell
3kjS@htb[/htb]$ ls -l /usr/share/hashcat/rules
```

```shell
3kjS@htb[/htb]$ hashcat -a 0 -m 0 1b0556a75770563578569ae21392630c /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule
```
#### Mask attack
|Symbol|Charset|
|---|---|
|?l|abcdefghijklmnopqrstuvwxyz|
|?u|ABCDEFGHIJKLMNOPQRSTUVWXYZ|
|?d|0123456789|
|?h|0123456789abcdef|
|?H|0123456789ABCDEF|
|?s|«space»!"#$%&'()*+,-./:;<=>?@[]^_`{|
|?a|?l?u?d?s|
|?b|0x00 - 0xff|

```shell
3kjS@htb[/htb]$ hashcat -a 3 -m 0 1e293d6912d074c0fd15844d803400dd '?u?l?l?l?l?d?s'
```

