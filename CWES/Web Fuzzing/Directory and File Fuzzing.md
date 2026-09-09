## Uncovering Hidden Assets

- `Sensitive data`: Backup files, configuration settings, or logs containing user credentials or other confidential information.
- `Outdated content`: Older versions of files or scripts that may be vulnerable to known exploits.
- `Development resources`: Test environments, staging sites, or administrative panels that could be leveraged for further attacks.
- `Hidden functionalities`: Undocumented features or endpoints that could expose unexpected vulnerabilities.
## Wordlists

`SecLists` contains wordlists for:

- Common directory and file names
- Backup files
- Configuration files
- Vulnerable scripts
- And much more

The most commonly used wordlists for fuzzing web directories and files from `SecLists` are:

- `Discovery/Web-Content/common.txt`: This general-purpose wordlist contains a broad range of common directory and file names on web servers. It's an excellent starting point for fuzzing and often yields valuable results.
- `Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt`: This is a more extensive wordlist specifically focused on directory names. It's a good choice when you need a deeper dive into potential directories.
- `Discovery/Web-Content/raft-large-directories.txt`: This wordlist boasts a massive collection of directory names compiled from various sources. It's a valuable resource for thorough fuzzing campaigns.
- `Discovery/Web-Content/big.txt`: As the name suggests, this is a massive wordlist containing both directory and file names. It's useful when you want to cast a wide net and explore all possibilities.
## Actually Fuzzing
### ffuf

We will use `ffuf` for this fuzzing task. Here's how `ffuf` generally works:

1. `Wordlist`: You provide `ffuf` with a wordlist containing potential directory or file names.
2. `URL with FUZZ keyword`: You construct a URL with the `FUZZ` keyword as a placeholder where the wordlist entries will be inserted.
3. `Requests`: `ffuf` iterates through the wordlist, replacing the `FUZZ` keyword in the URL with each entry and sending HTTP requests to the target web server.
4. `Response Analysis`: `ffuf` analyzes the server's responses (status codes, content length, etc.) and filters the results based on your criteria.

For example, if you want to fuzz for directories, you might use a URL like this:

```http
http://localhost/FUZZ
```
### Directory Fuzzing

```shell
3kjS@htb[/htb]$ ffuf -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -u http://IP:PORT/FUZZ
```
![](Screenshot%202026-09-08%20at%2013.36.17.png)
### File Fuzzing

- `.php`: Files containing PHP code, a popular server-side scripting language.
- `.html`: Files that define the structure and content of web pages.
- `.txt`: Plain text files, often storing simple information or logs.
- `.bak`: Backup files are created to preserve previous versions of files in case of errors or modifications.
- `.js`: Files containing JavaScript code add interactivity and dynamic functionality to web pages.

Utilize `ffuf` and a wordlist of common file names to search for hidden files with specific extensions:

```shell
3kjS@htb[/htb]$ ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://IP:PORT/w2ksvrus/FUZZ -e .php,.html,.txt,.bak,.js -v
```
![](Screenshot%202026-09-08%20at%2013.37.48.png)
