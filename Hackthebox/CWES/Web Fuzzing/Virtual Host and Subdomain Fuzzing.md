|Feature|Virtual Hosts|Subdomains|
|---|---|---|
|Identification|Identified by the `Host` header in HTTP requests.|Identified by DNS records, pointing to specific IP addresses.|
|Purpose|Primarily used to host multiple websites on a single server.|Used to organize different sections or services within a website.|
|Security Risks|Misconfigured vhosts can expose internal applications or sensitive data.|Subdomain takeover vulnerabilities can occur if DNS records are mismanaged.|
## Gobuster

`Gobuster's` flexibility extends to fuzzing for various types of content:

- `Directories`: Discover hidden directories on a web server.
- `Files`: Identify files with specific extensions (e.g., `.php`, `.txt`, `.bak`).
- `Subdomains`: Enumerate subdomains of a given domain.
- `Virtual Hosts (vhosts)`: Uncover hidden virtual hosts by manipulating the `Host` header.
### Gobuster VHost Fuzzing

```shell
3kjS@htb[/htb]$ echo "IP inlanefreight.htb" | sudo tee -a /etc/hosts
```

```sh
3kjS@htb[/htb]$ gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain
```

Running the command will execute a `vhost scan` against the target:

```sh
3kjS@htb[/htb]$ gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain
```
### Gobuster Subdomain Fuzzing

Let's break down the `Gobuster` subdomain fuzzing command:

```sh
3kjS@htb[/htb]$ gobuster dns -d inlanefreight.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

- `gobuster dns`: Activates `Gobuster's` DNS fuzzing mode, directing it to focus on discovering subdomains.
- `-d inlanefreight.com`: Specifies the target domain (e.g., `inlanefreight.com`) for which you want to discover subdomains.
- `-w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt`: This points to the wordlist file that `Gobuster` will use to generate potential subdomain names. In this example, we're using a wordlist containing the top 5000 most common subdomains.

Running this command, `Gobuster` might produce output similar to:

```sh
3kjS@htb[/htb]$ gobuster dns -d inlanefreight.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```
