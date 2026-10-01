# [Gitlab - Discovery & Enumeration](Gitlab%20-%20Discovery%20&%20Enumeration.md) | [Attacking GitLab](Attacking%20GitLab.md)
### What is the URL of the WordPress instance?

```shell
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://10.129.201.90 -H "Host:FUZZ.inlanefreight.local" -mc 200 -fs 0 -fl 923
```
![](Screenshot%202026-08-03%20at%2014.27.34.png)
`http://blog.inlanefreight.local`
### What is the name of the public GitLab project?

Moved to explore project and i saw Virtualhost project
![](Screenshot%202026-08-03%20at%2014.33.41.png)
### What is the FQDN of the third vhost?

Scrolled down the `README.md` and i saw monitoring as a Vhost that Admin of the project used
![](Screenshot%202026-08-03%20at%2014.35.29.png)
### What application is running on this third vhost? (One word)

Let's add it into /etc/hosts
![](Screenshot%202026-08-03%20at%2014.37.31.png)

It is Nagios
![](Screenshot%202026-08-03%20at%2014.39.29.png)
### What is the admin password to access this application?

I saw Nagios PostgreSQL in gitlab, let's move into it and enumerate
![](Screenshot%202026-08-03%20at%2014.40.23.png)

Last commit say `Update INSTALL with master password`
![](Screenshot%202026-08-03%20at%2014.41.51.png)
![](Screenshot%202026-08-03%20at%2014.42.26.png)
### Obtain reverse shell access on the target and submit the contents of the flag.txt file.

Nagios 5.7.5 is the version
![](Screenshot%202026-08-03%20at%2014.47.42.png)

Let check the Module from Metasploit
![](Screenshot%202026-08-03%20at%2014.53.17.png)
I'd use #4

Set LHOST, RHOST and PASSWORD that's it
![](Screenshot%202026-08-03%20at%2014.58.22.png)

DONE
![](Screenshot%202026-08-03%20at%2015.03.00.png)
