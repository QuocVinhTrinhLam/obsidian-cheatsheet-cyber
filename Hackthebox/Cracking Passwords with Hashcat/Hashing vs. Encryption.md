## Hashing

Let's consider the plaintext password value "p@ssw0rd". The MD5 hash for this can be calculated as follows:

```sh
3kjS@htb[/htb]$ echo -n "p@ssw0rd" | md5sum

0f359740bd1cda994f8b55330c86d845
```

Now, suppose a random salt such as "123456" is introduced and appended to the plaintext.

```sh
3kjS@htb[/htb]$ echo -n "p@ssw0rd123456" | md5sum

f64c413ca36f5cfe643ddbec4f7d92d0
```
## Encryption
## Symmetric Encryption

```sh
3kjS@htb[/htb]$ python3

Python 3.8.3 (default, May 14 2020, 11:03:12) 
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from pwn import xor
>>> xor("p@ssw0rd", "secret")
b'\x03%\x10\x01\x12D\x01\x01'
```

In the image above, the plaintext is `p@ssw0rd,` and the key is `secret`. Anyone who has the key can decrypt the ciphertext and obtain the plaintext.

```sh
3kjS@htb[/htb]$ python3

Python 3.8.3 (default, May 14 2020, 11:03:12) 
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from pwn import xor
>>> xor('\x03%\x10\x01\x12D\x01\x01', "secret")
b'p@ssw0rd'
```
## Asymmetric Encryption

On the other hand, asymmetric algorithms divide the key into two parts (i.e., public and private). The public key can be given to anyone who wishes to encrypt some information and pass it securely to the owner. The owner then uses their private key to decrypt the content. Some examples of asymmetric algorithms are [RSA](https://en.wikipedia.org/wiki/RSA_\(cryptosystem\)), [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm), and [Diffie-Hellman](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange).

One of the prominent uses of asymmetric encryption is the `Hypertext Transfer Protocol Secure` (`HTTPS`) protocol in the form of `Secure Sockets Layer` (`SSL`). When a client connects to a server hosting an `HTTPS` website, a public key exchange occurs. The client's browser uses this public key to encrypt any kind of data sent to the server. The server decrypts the incoming traffic before passing it on to the processing service.