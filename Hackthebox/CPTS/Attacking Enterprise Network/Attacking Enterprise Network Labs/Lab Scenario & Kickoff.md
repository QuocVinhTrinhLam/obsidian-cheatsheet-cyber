# [Scenario & Kickoff](Scenario%20&%20Kickoff.md)
### Perform a banner grab of the services listening on the target host and find a non-standard service banner. Submit the name as your answer (format: word_word_word)

```shell
sudo nmap --open -p- -A -oA inlanefreight_ept_tcp_all_svc -iL scope
```
![](Screenshot%202026-08-30%20at%2015.18.40.png)
### Perform a DNS Zone Transfer against the target and find a flag. Submit the flag value as your answer (flag format: HTB{ }).

```shell
dig axfr inlanefreight.local @10.129.229.147
```
![](Screenshot%202026-08-30%20at%2015.20.22.png)
### What is the FQDN of the associated subdomain?

![](Screenshot%202026-08-30%20at%2015.33.12.png)
### Perform vhost discovery. What additional vhost exists? (one word)

```shell
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ \
     -u http://10.129.229.147/ \
     -H 'Host: FUZZ.inlanefreight.local' \
     -fs 15157
```
![](Screenshot%202026-08-30%20at%2015.37.27.png)
