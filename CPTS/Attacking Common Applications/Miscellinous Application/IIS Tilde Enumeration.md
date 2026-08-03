## Enumeration
#### Nmap - Open ports

```shell
3kjS@htb[/htb]$ nmap -p- -sV -sC --open 10.129.224.91
```
![](Screenshot%202026-08-02%20at%2021.23.31.png)
#### Tilde Enumeration using IIS ShortName Scanner

```shell
3kjS@htb[/htb]$ java -jar iis_shortname_scanner.jar 0 5 http://10.129.204.231/
```
![](Screenshot%202026-08-02%20at%2021.23.49.png)
#### Generate Wordlist

```shell
3kjS@htb[/htb]$ egrep -r ^transf /usr/share/wordlists/* | sed 's/^[^:]*://' > /tmp/list.txt
```

| **Command Part**    | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `egrep -r ^transf`  | The `egrep` command is used to search for lines containing a specific pattern in the input files. The `-r` flag indicates a recursive search through directories. The `^transf` pattern matches any line that starts with "transf". The output of this command will be lines that begin with "transf" along with their source file names.                                                                                           |
| `\|`                | The pipe symbol (`\|`) is used to pass the output of the first command (`egrep`) to the second command (`sed`). In this case, the lines starting with "transf" and their file names will be the input for the `sed` command.                                                                                                                                                                                                        |
| `sed 's/^[^:]*://'` | The `sed` command is used to perform a find-and-replace operation on its input (in this case, the output of `egrep`). The `'s/^[^:]*://'` expression tells `sed` to find any sequence of characters at the beginning of a line (`^`) up to the first colon (`:`), and replace them with nothing (effectively removing the matched text). The result will be the lines starting with "transf" but without the file names and colons. |
| `> /tmp/list.txt`   | The greater-than symbol (`>`) is used to redirect the output of the entire command (i.e., the modified lines) to a new file named `/tmp/list.txt`.                                                                                                                                                                                                                                                                                  |
#### Gobuster Enumeration

```shell
3kjS@htb[/htb]$ gobuster dir -u http://10.129.204.231/ -w /tmp/list.txt -x .aspx,.asp
```
![](Screenshot%202026-08-02%20at%2021.24.59.png)
