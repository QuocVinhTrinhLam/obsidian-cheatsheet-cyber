## Vhosts vs. Sub-domains

``VHosts may or may not have public DNS records.``
## Vhosts Fuzzing

```shell
3kjS@htb[/htb]$ ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb'
```
