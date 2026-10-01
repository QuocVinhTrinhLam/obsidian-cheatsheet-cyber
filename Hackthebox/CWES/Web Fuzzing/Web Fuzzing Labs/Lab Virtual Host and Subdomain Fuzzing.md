# [Virtual Host and Subdomain Fuzzing](Virtual%20Host%20and%20Subdomain%20Fuzzing.md)
### Using GoBuster against the target system to fuzz for vhosts using the common.txt wordlist, which vhost starts with the prefix "web-"? Respond with the full vhost, eg web-123.inlanefreight.htb.

```sh
echo "154.57.164.73 inlanefreight.htb" | sudo tee -a /etc/hosts
```

```sh
sudo gobuster vhost -u http://inlanefreight.htb:30593 -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain
```
![](Screenshot%202026-09-09%20at%2012.45.43.png)
### Using GoBuster against inlanefreight.com to fuzz for subdomains using the subdomains-top1million-5000.txt wordlist, which subdomain starts with the prefix "su"? Respond with the full vhost, eg web.inlanefreight.com.

```sh
gobuster dns -d inlanefreight.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```
![](Screenshot%202026-09-09%20at%2012.56.39.png)