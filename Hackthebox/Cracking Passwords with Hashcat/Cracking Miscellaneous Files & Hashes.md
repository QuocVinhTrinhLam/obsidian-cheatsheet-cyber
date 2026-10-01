## Tools
#### JohnTheRipper - Installation

```sh
3kjS@htb[/htb]$ sudo git clone https://github.com/magnumripper/JohnTheRipper.git
3kjS@htb[/htb]$ cd JohnTheRipper/src
3kjS@htb[/htb]$ sudo ./configure && sudo make
```
## Example 1 - Cracking Password Protected Microsoft Office Documents

|**Mode**|**Target**|
|---|---|
|`9400`|MS Office 2007|
|`9500`|MS Office 2010|
|`9600`|MS Office 2013|
#### Extract Hash

```sh
3kjS@htb[/htb]$ python office2john.py hashcat_Word_example.docx 

hashcat_Word_example.docx:$office$*2013*100000*256*16*6e059661c3ed733f5730eaabb41da13a*aa38e007ee01c07e4fe95495934cf68f*2f1e2e9bf1f0b320172cd667e02ad6be1718585b6594691907b58191a6
```
#### Hashcat - Cracking MS Office Passwords

```sh
3kjS@htb[/htb]$ hashcat -m 9600 office_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```
## Example 2 - Cracking Password Protected Zip Files

| **Mode** | **Target**                                  |
| -------- | ------------------------------------------- |
| `11600`  | 7-Zip                                       |
| `13600`  | WinZip                                      |
| `17200`  | PKZIP (Compressed)                          |
| `17210`  | PKZIP (Uncompressed)                        |
| `17220`  | PKZIP (Compressed Multi-File)               |
| `17225`  | PKZIP (Mixed Multi-File)                    |
| `17230`  | PKZIP (Compressed Multi-File Checksum-Only) |
| `23001`  | SecureZIP AES-128                           |
| `23002`  | SecureZIP AES-192                           |
| `23003`  | SecureZIP AES-256                           |
#### Set Password for a ZIP File

```sh
3kjS@htb[/htb]$ zip --password zippyzippy blueprints.zip dummy.pdf 

adding: dummy.pdf (deflated 7%)
```
#### Extract Hash

```sh
3kjS@htb[/htb]$ zip2john ~/Desktop/HTB/Academy/Cracking\ with\ Hashcat/blueprints.zip
```
#### Hashcat - Cracking ZIP Files

```sh
3kjS@htb[/htb]$ hashcat -a 0 -m 17200 pdf_hash_to_crack /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```
## Example 3 - Cracking Password Protected KeePass Files

| **Mode** | **Target**                       |
| -------- | -------------------------------- |
| `13400`  | KeePass 1 AES / without keyfile  |
| `13400`  | KeePass 2 AES / without keyfile  |
| `13400`  | KeePass 1 Twofish / with keyfile |
| `13400`  | Keepass 2 AES / with keyfile     |
#### Extract Hash

```sh
3kjS@htb[/htb]$ python keepass2john.py Master.kdbx 

Master:$keepass$*2*60000*222*d14132325949a3b4efacdb2e729ec54403308c85654fe4ababccfb8ddc185d09*5c09bed9c98f8ee08aa7a71fe735b30849ec87e6cb7f1caa96d606ce9f077f7e*bd372d79d8aceea9689ad49428b8efde*28d21caedf25617db0833bd721a42c963e874e0b9fbe7fe1187a4a8ecb3b1d19*a539abd3cfd7ee5982fa28c44dd226ce05a1102d04a5f590eabf5138cd2a6403
```
#### Hashcat - Cracking KeePass Files

```sh
3kjS@htb[/htb]$ hashcat -a 0 -m 13400 keepass_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```
## Example 4 - Cracking Protected PDF Files
#### Extract Hash

```sh
3kjS@htb[/htb]$ python pdf2john.py inventory.pdf | awk -F":" '{ print $2}'

$pdf$4*4*128*-1028*1*16*f7d77b3d22b9f92829d49ff5d78b8f28*32*d33f35f776215527d65155f79d9ed79800000000000000000000000000000000*32*6cfb859c107acaae8c0ca9ceec56fd91ff75fe7b1cddb03f629ca3583f59e52f
```

|**Mode**|**Target**|
|---|---|
|`10400`|PDF 1.1 - 1.3 (Acrobat 2 - 4)|
|`10410`|PDF 1.1 - 1.3 (Acrobat 2 - 4), collider #1|
|`10420`|PDF 1.1 - 1.3 (Acrobat 2 - 4), collider #2|
|`10500`|PDF 1.4 - 1.6 (Acrobat 5 - 8)|
|`10600`|PDF 1.7 Level 3 (Acrobat 9)|
|`10700`|PDF 1.7 Level 8 (Acrobat 10 - 11)|
#### Hashcat - Cracking PDF Files

```sh
3kjS@htb[/htb]$ hashcat -a 0 -m 10500 pdf_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```
