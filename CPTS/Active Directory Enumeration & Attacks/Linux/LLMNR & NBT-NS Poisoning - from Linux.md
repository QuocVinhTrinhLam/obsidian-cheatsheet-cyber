## TTPs
|**Tool**|**Description**|
|---|---|
|[Responder](https://github.com/lgandx/Responder)|Responder is a purpose-built tool to poison LLMNR, NBT-NS, and MDNS, with many different functions.|
|[Inveigh](https://github.com/Kevin-Robertson/Inveigh)|Inveigh is a cross-platform MITM platform that can be used for spoofing and poisoning attacks.|
|[Metasploit](https://www.metasploit.com/)|Metasploit has several built-in scanners and spoofing modules made to deal with poisoning attacks.|
### Responder In Action
#### Responder Logs
![[Screenshot 2026-05-23 at 16.57.04.png]]
#### Starting Responder with Default Settings

```shell
sudo responder -I ens224
```
#### Cracking an NTLMv2 Hash With Hashcat

```shell
3kjS@htb[/htb]$ hashcat -m 5600 forend_ntlmv2 /usr/share/wordlists/rockyou.txt
```
