| **Mask** | **Description**                      | **Example**                       |
| -------- | ------------------------------------ | --------------------------------- |
| `?l`     | Lower-case ASCII letters             | abcdefghijklmnopqrstuvwxyz        |
| `?u`     | Upper-case ASCII letters             | ABCDEFGHIJKLMNOPQRSTUVWXYZ        |
| `?d`     | Digits                               | 0123456789                        |
| `?h`     | Digits with lower-case ASCII letters | 0123456789abcdef                  |
| `?H`     | Digits with upper-case ASCII letters | 0123456789ABCDEF                  |
| `?s`     | Special characters                   | «space»!"#$%&'()*+,-./:;<=>?@\^_{ |
| `?a`     | Combination of ?l, ?u, ?d and ?s     | ?l?u?d?s                          |
| `?b`     | All possible byte values             | ?l?u?d?s                          |
## Fixed Length Brute Forcing

```sh
3kjS@htb[/htb]$ hashcat -m 22000 -a 3 wpa_hash '?u?l?l?l?l?l?l?l?a?d?d?d?d?d'
```
## Variable Length Brute Forcing
#### Mask Increments

```sh
3kjS@htb[/htb]$ hashcat -a 3 -m 22000 wpa_hash --increment --increment-min 8 --increment-max=14 '?u?l?l?l?l?l?l?l?d?d?d?d?d?d'
```

Let's quickly summarize each of the command-line flags:

- `-m 22000`: Specifies the WPA2-PMKID/EAPOL hash type.
- `-a 3`: Specifies a mask attack.
- `--increment`: Enables incremental guessing.
- `--increment-min=8` : Starts guessing from 8 characters.
- `--increment-max=14`: Stops at 14 characters.
## Custom Character Sets

```sh
3kjS@htb[/htb]$ hashcat -a 3 -m 22000 wpa_hash --increment --increment-min 8 --increment-max=14 -1 ?d?s '?u?u?u?u?u?u?u?u?1?1?1?1?1'
```

Suppose we define two custom character sets:

- Set `?1`: Uppercase and lowercase letters, plus digits  
    (`abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789`)
- Set `?2`: Lowercase letters, special characters, and digits  
    (`abcdefghijklmnopqrstuvwxyz!@#$%&*-+0123456789`)
#### Creating and Using .hcchr Files

```sh
3kjS@htb[/htb]$ echo 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789' > starterchars.hcchr
3kjS@htb[/htb]$ echo 'abcdefghijklmnopqrstuvwxyz!@#$%&*-+0123456789' > endchars.hcchr
```

```sh
3kjS@htb[/htb]$ hashcat -a 3 -m 22000 wpa_hash --increment --increment-min=8 --increment-max=12 -1 starterchars.hcchr -2 endchars.hcchr ?1?1?1?1?1?1?2?2?2?2?2?2
```
#### Using Multiple Custom Charsets

If we want to define and use multiple custom character sets in a Hashcat mask attack, we can use the `-1`, `-2`, `-3`, and `-4` switches together to assign different sets.

- -1 abcdefghijklmn?d
- -2 ?H?s
- -3 ?dabcdef
- -4 charsets/special/Russian/ru_ISO-8859-5-special.hcchr

```sh
3kjS@htb[/htb]$ hashcat -a 3 -m 22000 wpa_hash -1 abcdefghijklmn?d -2 ?H?s -3 ?dabcdefghi -4 charsets/special/Russian/ru_ISO-8859-5-special.hcchr ?1?2?3?4?1?2?3?4
```
