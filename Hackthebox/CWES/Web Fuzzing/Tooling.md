### Installing Go, Python and PIPX

Open a terminal and update your package lists to ensure you have the latest information on the newest versions of packages and their dependencies.

```shell
3kjS@htb[/htb]$ sudo apt update
```

Use the following command to install Go:

```shell
3kjS@htb[/htb]$ sudo apt install -y golang
```

Use the following command to install Python:

```shell
3kjS@htb[/htb]$ sudo apt install -y python3 python3-pip
```

Use the following command to install and configure pipx:

```shell
3kjS@htb[/htb]$ sudo apt install pipx
3kjS@htb[/htb]$ pipx ensurepath
3kjS@htb[/htb]$ sudo pipx ensurepath --global
```

To ensure that Go and Python are installed correctly, you can check their versions:

```shell
3kjS@htb[/htb]$ go version 
3kjS@htb[/htb]$ python3 --version
```
## FFUF

```shell
3kjS@htb[/htb]$ go install github.com/ffuf/ffuf/v2@latest
```
### Use Cases

|Use Case|Description|
|---|---|
|`Directory and File Enumeration`|Quickly identify hidden directories and files on a web server.|
|`Parameter Discovery`|Find and test parameters within web applications.|
|`Brute-Force Attack`|Perform brute-force attacks to discover login credentials or other sensitive information.|
## Gobuster

```shell
3kjS@htb[/htb]$ go install github.com/OJ/gobuster/v3@latest
```
### Use Cases

| Use Case                      | Description                                                                             |
| ----------------------------- | --------------------------------------------------------------------------------------- |
| `Content Discovery`           | Quickly scan and find hidden web content such as directories, files, and virtual hosts. |
| `DNS Subdomain Enumeration`   | Identify subdomains of a target domain.                                                 |
| `WordPress Content Detection` | Use specific wordlists to find WordPress-related content.                               |
## FeroxBuster

```shell
3kjS@htb[/htb]$ curl -sL https://raw.githubusercontent.com/epi052/feroxbuster/main/install-nix.sh | sudo bash -s $HOME/.local/bin
```
### Use Cases

|Use Case|Description|
|---|---|
|`Recursive Scanning`|Perform recursive scans to discover nested directories and files.|
|`Unlinked Content Discovery`|Identify content that is not linked within the web application.|
|`High-Performance Scans`|Benefit from Rust's performance to conduct high-speed content discovery.|
## wfuzz/wenum

```shell
3kjS@htb[/htb]$ pipx install git+https://github.com/WebFuzzForge/wenum 
3kjS@htb[/htb]$ pipx runpip wenum install setuptools
```
### Use Cases

|Use Case|Description|
|---|---|
|`Directory and File Enumeration`|Quickly identify hidden directories and files on a web server.|
|`Parameter Discovery`|Find and test parameters within web applications.|
|`Brute-Force Attack`|Perform brute-force attacks to discover login credentials or other sensitive information.|
