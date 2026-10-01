![](CRC32%20Generation%20(WEP's%20ICV%20Algorithm)-20260921-141241.png)

We can calculate the CRC32 checksum using the Python [zlib](https://docs.python.org/3/library/zlib.html#zlib.crc32) library. With the script below, we will take our packet plaintext 'Something Sensitive' and find the checksum value for it.

```python
import zlib

# First we declare our packet plaintext. In normal communications this is the actual plaintext data.
packetplaintext = b'Something Sensitive'

# We then use the zlib library to calculate the CRC32.
crc32 = zlib.crc32(packetplaintext)

print(crc32)
```

```python
3kjS@htb[/htb]$ python3 CRC32.py

2950664974
```
