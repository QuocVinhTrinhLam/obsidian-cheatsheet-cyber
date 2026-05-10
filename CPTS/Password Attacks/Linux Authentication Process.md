## Passwd file

```shell
htb-student:x:1000:1000:,,,:/home/htb-student:/bin/bash
```

|Field|Value|
|---|---|
|Username|`htb-student`|
|Password|`x`|
|User ID|`1000`|
|Group ID|`1000`|
|[GECOS](https://en.wikipedia.org/wiki/Gecos_field)|`,,,`|
|Home directory|`/home/htb-student`|
|Default shell|`/bin/bash`|

```shell
3kjS@htb[/htb]$ head -n 1 /etc/passwd 

root::0:0:root:/root:/bin/bash
```
## Shadow file

```shell
htb-student:$y$j9T$3QSBB6CbHEu...SNIP...f8Ms:18955:0:99999:7:::
```

|Field|Value|
|---|---|
|Username|`htb-student`|
|Password|`$y$j9T$3QSBB6CbHEu...SNIP...f8Ms`|
|Last change|`18955`|
|Min age|`0`|
|Max age|`99999`|
|Warning period|`7`|
|Inactivity period|`-`|
|Expiration date|`-`|
|Reserved field|`-`|

- `$<id>$<salt>$<hashed>`

| ID     | Cryptographic Hash Algorithm                                          |
| ------ | --------------------------------------------------------------------- |
| `1`    | [MD5](https://en.wikipedia.org/wiki/MD5)                              |
| `2a`   | [Blowfish](https://en.wikipedia.org/wiki/Blowfish_\(cipher\))         |
| `5`    | [SHA-256](https://en.wikipedia.org/wiki/SHA-2)                        |
| `6`    | [SHA-512](https://en.wikipedia.org/wiki/SHA-2)                        |
| `sha1` | [SHA1crypt](https://en.wikipedia.org/wiki/SHA-1)                      |
| `y`    | [Yescrypt](https://github.com/openwall/yescrypt)                      |
| `gy`   | [Gost-yescrypt](https://www.openwall.com/lists/yescrypt/2019/06/30/1) |
| `7`    | [Scrypt](https://en.wikipedia.org/wiki/Scrypt)                        |

## Opasswd

```shell
3kjS@htb[/htb]$ sudo cat /etc/security/opasswd 

cry0l1t3:1000:2:$1$HjFAfYTG$qNDkF0zJ3v8ylCOrKB0kt0,$1$kcUjWZJX$E9uMSmiQeRh4pAAgzuvkq1
```
## Cracking Linux Credentials

```shell
3kjS@htb[/htb]$ sudo cp /etc/passwd /tmp/passwd.bak 
3kjS@htb[/htb]$ sudo cp /etc/shadow /tmp/shadow.bak 
3kjS@htb[/htb]$ unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes
```

This "unshadowed" file can now be attacked with either JtR or hashcat.

```shell
3kjS@htb[/htb]$ hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked
```
