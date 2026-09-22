![](Seed%20Generation%20and%20the%20RC4%20Algorithm-20260921-140812.png)

With this example script, our goal is to encrypt the phrase 'Wired Equivalent Privacy' with both a 64-bit and 128-bit seed. First, we generate the random 3-byte initialization vector (IV) using `get_random_bytes`. We then concatenate the IV and Key together to make the full seed. The seed is passed into the two phases of the RC4 algorithm to create the keystream, which is then XORed with our 'Wired Equivalent Privacy Message'.

```python
import Crypto
from Crypto.Random import get_random_bytes
import binascii
from Crypto.Cipher import ARC4

# Generating the 24-bit (3 byte) Initialization Vector
IV = get_random_bytes(3)

# Creating the 40-bit key (5 bytes)
key = b'\x01\x02\x03\x04\x05'
Seed64 = IV + key

# We can also use a 104-bit key (13 bytes) 
key104 = b'\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0A\x0B\x0C\x0D'
Seed128 = IV + key104

print('Initialization Vector: ' + str(IV))
print('64-bit Seed: ' + str(Seed64))
print('128-bit Seed: ' + str(Seed128))

# We must use the RC4 cipher to encrypt the plain text. We will explore how to generate the CRC32 and ICV Message in the next session.
# The RC4 cipher consists of the Key-Scheduling Algorithm and the Pseudo-random Generation Algorithm, which outputs the keystream.

# Generating the keystream using RC4
keystream = ARC4.new(Seed64)
keystreamB = ARC4.new(Seed128)

# The plain text is XORed with the keystream to produce the ciphertext.
msg = keystream.encrypt(b'Wired Equivalent Privacy')
print(msg)
```

We can see the algorithm in action using the following command.

```sh
3kjS@htb[/htb]$ python3 SeedGen.py

Initialization Vector: b'y#K'
64-bit Seed: b'yK\x01\x02\x03\x04\x05'
128-bit Seed: b'yK\x01\x02\x03\x04\x05\x06\x07\x08\t\n\x0b\x0c\r'
b')c\xe96\xf0\xab\x10\x9b\xa2\x9f\xdd\x19\xff\xf5\x81\xd5\xe2\xe9-x\x16\x96%n'
```

```sh
3kjS@htb[/htb]$ python3 SeedGen.py

Initialization Vector: b'\xdb\x10o'
64-bit Seed: b'\xdb\x10o\x01\x02\x03\x04\x05'
128-bit Seed: b'\xdb\x10o\x01\x02\x03\x04\x05\x06\x07\x08\t\n\x0b\x0c\r'
b'\xf4kR\x06/3\x08 O\x9a\xa2\x99\x9a\x93\xe5\x16\x89\x9f\x7f\x92\x1d\xd1\x1b\xb7'
```
