## Using Plugins

```shell
3kjS@htb[/htb]$ ls /usr/share/metasploit-framework/plugins
```
#### MSF - Load Nessus

```shell
msf6 > load nessus

msf6 > nessus_help
```

If the plugin is not installed correctly, we will receive the following error upon trying to load it.

```shell
msf6 > load Plugin_That_Does_Not_Exist
```
## Installing new Plugins
#### Downloading MSF Plugins

```shell
3kjS@htb[/htb]$ git clone https://github.com/darkoperator/Metasploit-Plugins 
3kjS@htb[/htb]$ ls Metasploit-Plugins
```
#### MSF - Copying Plugin to MSF

```shell
3kjS@htb[/htb]$ sudo cp ./Metasploit-Plugins/pentest.rb /usr/share/metasploit-framework/plugins/pentest.rb
```
#### MSF - Load Plugin

```shell
3kjS@htb[/htb]$ msfconsole -q 
msf6 > load pentest
```

Check out the list of popular plugins below:

| [nMap (pre-installed)](https://nmap.org/)                                                                           | [NexPose (pre-installed)](https://sectools.org/tool/nexpose/)                                                                       | [Nessus (pre-installed)](https://www.tenable.com/products/nessus)                                               |
| ------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| [Mimikatz (pre-installed V.1)](http://blog.gentilkiwi.com/mimikatz)                                                 | [Stdapi (pre-installed)](https://www.rubydoc.info/github/rapid7/metasploit-framework/Rex/Post/Meterpreter/Extensions/Stdapi/Stdapi) | [Railgun](https://github.com/rapid7/metasploit-framework/wiki/How-to-use-Railgun-for-Windows-post-exploitation) |
| [Priv](https://github.com/rapid7/metasploit-framework/blob/master/lib/rex/post/meterpreter/extensions/priv/priv.rb) | [Incognito (pre-installed)](https://www.offensive-security.com/metasploit-unleashed/fun-incognito/)                                 | [Darkoperator's](https://github.com/darkoperator/Metasploit-Plugins)                                            |

