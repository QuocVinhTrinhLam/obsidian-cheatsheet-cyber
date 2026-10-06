| **Placeholder** | **Meaning**                                             |
| --------------- | ------------------------------------------------------- |
| ?l              | lower-case ASCII letters (a-z)                          |
| ?u              | upper-case ASCII letters (A-Z)                          |
| ?d              | digits (0-9)                                            |
| ?h              | 0123456789abcdef                                        |
| ?H              | 0123456789ABCDEF                                        |
| ?s              | special characters («space»!"#$%&'()*+,-./:;<=>?@[]^_`{ |
| ?a              | ?l?u?d?s                                                |
| ?b              | 0x00 - 0xff                                             |
#### Creating MD5 hashes

```sh
3kjS@htb[/htb]$ echo -n 'ILFREIGHTabcxy2015' | md5sum | tr -d " -" > md5_mask_example_hash
```
#### Hashcat - Mask Attack

```sh
3kjS@htb[/htb]$ hashcat -a 3 -m 0 md5_mask_example_hash -1 01 'ILFREIGHT?l?l?l?l?l20?1?d'
```
