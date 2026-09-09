
After doing the exercise in the previous section, we got a strange block of text that seems to be encoded:

```sh
3kjS@htb[/htb]$ curl http://SERVER_IP:PORT/serial.php -X POST -d "param1=sample" 

ZG8gdGhlIGV4ZXJjaXNlLCBkb24ndCBjb3B5IGFuZCBwYXN0ZSA7KQo=
```
## Base64
#### Spotting Base64

`base64` encoded strings are easily spotted since they only contain alpha-numeric characters. However, the most distinctive feature of `base64` is its padding using = characters. The length of `base64` encoded strings has to be in a multiple of 4. If the resulting output is only 3 characters long, for example, an extra = is added as padding, and so on.
#### Base64 Encode

To encode any text into `base64` in Linux, we can echo it and pipe it with '`|`' to `base64`:

```sh
3kjS@htb[/htb]$ echo https://www.hackthebox.eu/ | base64 

aHR0cHM6Ly93d3cuaGFja3RoZWJveC5ldS8K
```
#### Base64 Decode

If we want to decode any `base64` encoded string, we can use `base64 -d`, as follows:

```sh
3kjS@htb[/htb]$ echo aHR0cHM6Ly93d3cuaGFja3RoZWJveC5ldS8K | base64 -d 

https://www.hackthebox.eu/
```
## Hex
#### Spotting Hex

Any string encoded in `hex` would be comprised of hex characters only, which are 16 characters only: 0-9 and a-f. That makes spotting `hex` encoded strings just as easy as spotting `base64` encoded strings.
#### Hex Encode

To encode any string into `hex` in Linux, we can use the `xxd -p` command:

```sh
3kjS@htb[/htb]$ echo https://www.hackthebox.eu/ | xxd -p 

68747470733a2f2f7777772e6861636b746865626f782e65752f0a
```
#### Hex Decode

To decode a `hex` encoded string, we can use the `xxd -p -r` command:

```sh
3kjS@htb[/htb]$ echo 68747470733a2f2f7777772e6861636b746865626f782e65752f0a | xxd -p -r 

https://www.hackthebox.eu/
```
## Caesar/Rot13
#### Spotting Caesar/Rot13

Even though this encoding method makes any text looks random, it is still possible to spot it because each character is mapped to a specific character. For example, in `rot13`, `http://www` becomes `uggc://jjj`, which still holds some resemblances and may be recognized as such.
#### Rot13 Encode

```sh
echo https://www.hackthebox.eu/ | tr 'A-Za-z' 'N-ZA-Mn-za-m' 

uggcf://jjj.unpxgurobk.rh/
```
#### Rot13 Decode

```sh
echo uggcf://jjj.unpxgurobk.rh/ | tr 'A-Za-z' 'N-ZA-Mn-za-m' 

https://www.hackthebox.eu/
```
## Other Types of Encoding

There are hundreds of other encoding methods we can find online. Even though these are the most common, sometimes we will come across other encoding methods, which may require some experience to identify and decode.

`If you face any similar types of encoding, first try to determine the type of encoding, and then look for online tools to decode it.`

Some tools can help us automatically determine the type of encoding, like [Cipher Identifier](https://www.boxentriq.com/code-breaking/cipher-identifier). Try the encoded strings above with [Cipher Identifier](https://www.boxentriq.com/code-breaking/cipher-identifier), to see if it can correctly identify the encoding method.

Other than encoding, many obfuscation tools utilize encryption, which is encoding a string using a key, which may make the obfuscated code very difficult to reverse engineer and deobfuscate, especially if the decryption key is not stored within the script itself.