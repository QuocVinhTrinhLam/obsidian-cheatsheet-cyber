[Sliver](https://github.com/BishopFox/sliver) is developed and actively updated by [BishopFox](https://bishopfox.com/). The installation we are going to follow throughout the module focuses on utilizing the pre-compiled binaries, each having their separate responsibilities. `Sliver`'s `Server` component has the important role of serving as the location implants will communicate back to, and `Sliver`'s `Client` component has the role of being the location the user will execute the commands and tools needed to fulfill their objectives. Sliver can also be installed using a [linux one-liner](https://github.com/BishopFox/sliver?tab=readme-ov-file#linux-one-liner) script. One of the drawbacks of using that approach is one can accidently use the `Ctrl + C` key combination, and risk losing every callback previously established. A callback is the term used when an implant has been executed on the target system and is actively communicating back to the server.
## Installation
#### Server setup

```sh
3kjS@htb[/htb]$ wget -q https://github.com/BishopFox/sliver/releases/download/v1.5.42/sliver-server_linux
3kjS@htb[/htb]$ chmod +x ./sliver-server_linux
```

```sh
3kjS@htb[/htb]$ ./sliver-server_linux
```
![](Screenshot%202026-11-02%20at%2014.11.35.png)
#### Operator profile

```sh
[server] sliver > new-operator -n student -l 10.10.14.193

[*] Generating new client certificate, please wait ... 
[*] Saved new client config to: /home/htb-ac-8414/student_10.10.14.193.cfg
```
#### Multiplayer mode

```sh
[server] sliver > multiplayer

[*] Multiplayer mode enabled!
[*] student has joined the game
```
#### Client setup

```sh
3kjS@htb[/htb]$ wget -q https://github.com/BishopFox/sliver/releases/download/v1.5.42/sliver-client_linux
3kjS@htb[/htb]$ chmod +x ./sliver-client_linux
3kjS@htb[/htb]$ ./sliver-client_linux import student_10.10.14.193.cfg 
2023/09/27 11:45:03 Saved new client config to: /home/htb-ac-8414/.sliver-client/configs/student_10.10.14.193.cfg
```

```sh
3kjS@htb[/htb]$ ./sliver-client_linux 
Connecting to 10.10.14.193:31337 ...

.------..------..------..------..------..------.
|S.--. ||L.--. ||I.--. ||V.--. ||E.--. ||R.--. |
| :/\: || :/\: || (/) || :(): || (/) || :(): |
| :/: || (__) || :/: || ()() || :/: || ()() |
| '--'S|| '--'L|| '--'I|| '--'V|| '--'E|| '--'R|
`------'`------'`------'`------'`------'`------'

All hackers gain scavenge
[*] Server v1.5.41 - f2a3915c79b31ab31c0c2f0428bbd53d9e93c54b
[*] Welcome to the sliver shell, please type 'help' for options

[*] Check for updates with the 'update' command

sliver >
```
## Armory
#### Armory installation

The extensions (utilities) in the armory can be installed using the [armory](https://github.com/BishopFox/sliver/blob/master/docs/sliver-docs/pages/docs/md/Armory.md) command followed by a subcommand such as `install`, `search` and `update`.

![](Screenshot%202026-11-02%20at%2014.13.21.png)

```sh
sliver > armory install seatbelt

[*] Installing alias 'Seatbelt' (v0.0.4) ... done!
```

```sh
sliver > armory install all
```
