## Common Hash Types
| Hashmode | Hash Name                      | Example Hash                                                                                                                                                                   |
| -------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0        | MD5                            | 8743b52063cd84097a65d1633f5c74f5                                                                                                                                               |
| 100      | SHA1                           | b89eaac7e61417341b710b727768294d0e6a277b                                                                                                                                       |
| 1000     | NTLM                           | b4b9b02e6f09a9bd760f388b67351e2b                                                                                                                                               |
| 1800     | sha512crypt $6$, SHA512 (Unix) | $6$52450745$k5ka2p8bFuSmoVT1tzOyyuaREkkKBcCNqoDKzYiJL9RaE8yMnPgh2XzzF0NDrUhgrcLwg78xs1w5pJiypEdFX/                                                                             |
| 3200     | bcrypt $2*$, Blowfish (Unix)   | $2a$05$LhayLxezLhK1LhWvKxCyLOj0j1u.Kj0jZ0pEmm134uzrQlFvQJLF6                                                                                                                   |
| 5500     | NetNTLMv1 / NetNTLMv1+ESS      | u4-netntlm::kNS:338d08f8e26de93300000000000000000000000000000000:9526fb8c23a90751cdd619b6cea564742e1e4bf33006ba41:cb8086049ec4736c                                             |
| 5600     | NetNTLMv2                      | admin::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c7830310000000000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030 |
| 13100    | Kerberos 5 TGS-REP etype 23    | $krb5tgs$23$_user$realm$test/spn_$63386d22d359fe42230300d56852c9eb$ < SNIP >                                                                                                   |
## Example 1 - Database Dumps
#### SHA1 Hashes List

```sh
winter!
baseball1
waterslide
summertime
baconandeggs
beach1234
sunshine1
welcome1
password123
```
#### Generate SHA1 Hashes

```sh
3kjS@htb[/htb]$ for i in $(cat words); do echo -n $i | sha1sum | tr -d ' -';done

fa3c9ecfc251824df74026b4f40e4b373fd4fc46
e6852777c0260493de41fb43918ab07bbb3a659c
0c3feaa16f73493f998970e22b2a02cb9b546768
b863c49eada14e3a8816220a7ab7054c28693664
b0feedd70a346f7f75086026169825996d7196f9
f47f832cba913ec305b07958b41babe2e0ad0437
08b314f0e1e2c41ec92c3735910658e5a82c6ba7
e35bece6c5e6e0e86ca51d0440e92282a9d6ac8a
cbfdac6008f9cab4083784cbd1874f76618d2a97
```

```sh
3kjS@htb[/htb]$ hashcat -m 100 SHA1_hashes /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```
![](Screenshot%202026-09-29%20at%2016.28.13.png)
## Example 2 - Linux Shadow File
#### Root Password in Ubuntu Linux

```sh
root:$6$tOA0cyybhb/Hr7DN$htr2vffCWiPGnyFOicJiXJVMbk1muPORR.eRGYfBYUnNPUjWABGPFiphjIjJC5xPfFUASIbVKDAHS3vTW1qU.1:18285:0:99999:7:::
```

```sh
3kjS@htb[/htb]$ hashcat -m 1800 nix_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```
## Example 3 - Common Active Directory Password Hash Types
#### NTLM
#### Python3 - Hashlib

```sh
3kjS@htb[/htb]$ python3

Python 3.8.3 (default, May 14 2020, 11:03:12) 
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.

>>> import hashlib,binascii
>>> hash = hashlib.new('md4', "Password01".encode('utf-16le')).digest()
>>> print (binascii.hexlify(hash))

b'7100a909c7ff05b266af3c42ec058c33'
```
#### Hashcat - Cracking NTLM Hashes

```sh
3kjS@htb[/htb]$ hashcat -a 0 -m 1000 ntlm_example /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```
#### NetNTLMv2
#### Responder - NTLMv2

```sh
sqladmin::INLANEFREIGHT:f54d6f198a7a47d4:7FECABAE13101DAAA20F1B09F7F7A4EA:0101000000000000C0653150DE09D20126F3F71DF13C1FD8000000000200080053004D004200330001001E00570049004E002D00500052004800340039003200520051004100460056000400140053004D00420033002E006C006F00630061<SNIP>
```
#### Hashcat - Cracking NTLMv2 Hashes

```sh
3kjS@htb[/htb]$ hashcat -a 0 -m 5600 inlanefreight_ntlmv2 /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```
