# [Hashing vs. Encryption](Hashing%20vs.%20Encryption.md)
### Generate an MD5 hash of the password 'HackTheBox123!'.

![](Screenshot%202026-09-29%20at%2014.44.59.png)
### Create the XOR ciphertext of the password 'opens3same' using the key 'academy'. (Answer format: \x00\x00\x00\....)

```sh
┌──(sjke㉿kali)-[~]
└─$ python3                  
Python 3.13.12 (main, Feb  4 2026, 15:06:39) [GCC 15.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from pwn import xor
>>> xor("opens3same", "academy")
/usr/lib/python3/dist-packages/pwnlib/util/fiddling.py:340: BytesWarning: Text is not bytes; assuming ASCII, no guarantees. See https://docs.pwntools.com/#bytes
  strs = [packing.flat(s, word_size = 8, sign = False, endianness = 'little') for s in args]
b'\x0e\x13\x04\n\x16^\n\x00\x0e\x04'
```

![](Screenshot%202026-09-29%20at%2014.46.34.png)