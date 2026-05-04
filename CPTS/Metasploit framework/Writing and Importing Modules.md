#### MSF - Search for Exploits

```shell
msf6 > search nagios
```

```shell
3kjS@htb[/htb]$ searchsploit nagios3
```

```shell
3kjS@htb[/htb]$ searchsploit -t Nagios3 --exclude=".py"
```
#### MSF - Directory Structure

```shell
3kjS@htb[/htb]$ ls /usr/share/metasploit-framework/
```

```shell
3kjS@htb[/htb]$ ls .msf4/
```
#### MSF - Loading Additional Modules at Runtime

```shell
3kjS@htb[/htb]$ cp ~/Downloads/9861.rb /usr/share/metasploit-framework/modules/exploits/unix/webapp/nagios3_command_injection.rb 

3kjS@htb[/htb]$ msfconsole -m /usr/share/metasploit-framework/modules/
```
#### MSF - Loading Additional Modules

```shell
msf6> loadpath /usr/share/metasploit-framework/modules/
```

```shell
msf6 > reload_all msf6 > use exploit/unix/webapp/nagios3_command_injection msf6 exploit(unix/webapp/nagios3_command_injection) > show options
```

## Porting Over Scripts into Metasploit Modules
#### Porting MSF Modules

```shell
3kjS@htb[/htb]$ ls /usr/share/metasploit-framework/modules/exploits/linux/http/ | grep bludit 

bludit_upload_images_exec.rb
```

```shell
3kjS@htb[/htb]$ cp ~/Downloads/48746.rb /usr/share/metasploit-framework/modules/exploits/linux/http/bludit_auth_bruteforce_mitigation_bypass.rb
```

