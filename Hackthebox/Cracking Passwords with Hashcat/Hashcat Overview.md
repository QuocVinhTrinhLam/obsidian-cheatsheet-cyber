|**#**|**Mode**|
|---|---|
|0|Straight|
|1|Combination|
|3|Brute-force|
|6|Hybrid Wordlist + Mask|
|7|Hybrid Mask + Wordlist|
#### Hashcat - Example Hashes

```sh
3kjS@htb[/htb]$ hashcat --example-hashes | less

hashcat (v6.1.1) starting...

MODE: 0
TYPE: MD5
HASH: 8743b52063cd84097a65d1633f5c74f5
PASS: hashcat

MODE: 10
TYPE: md5($pass.$salt)
HASH: 3d83c8e717ff0e7ecfe187f088d69954:343141
PASS: hashcat

MODE: 11
TYPE: Joomla < 2.5.18
HASH: b78f863f2c67410c41e617f724e22f34:89384528665349271307465505333378
PASS: hashcat

MODE: 12
TYPE: PostgreSQL
HASH: 93a8cf6a7d43e3b5bcd2dc6abb3e02c6:27032153220030464358344758762807
PASS: hashcat

MODE: 20
TYPE: md5($salt.$pass)
HASH: 57ab8499d08c59a7211c77f557bf9425:4247
PASS: hashcat

<SNIP>
```
#### Hashcat - Benchmark

```sh
3kjS@htb[/htb]$ hashcat -b -m 0
```
#### Hashcat - Optimizations

Hashcat has two main ways to optimize speed:

| Option            | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Optimized Kernels | This is the `-O` flag, which according to the documentation, means `Enable optimized kernels (limits password length)`. The magical password length number is generally 32, with most wordlists won't even hit that number. This can take the estimated time from days to hours, so it is always recommended to run with `-O` first and then rerun after without the `-O` if your GPU is idle. |
| Workload          | This is the `-w` flag, which, according to the documentation, means `Enable a specific workload profile`. The default number is `2`, but if you want to use your computer while Hashcat is running, set this to `1`. If you plan on the computer only running Hashcat, this can be set to `3`.                                                                                                 |