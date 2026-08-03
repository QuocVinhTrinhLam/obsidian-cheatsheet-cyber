# [Attacking Applications Connecting to Services](Attacking%20Applications%20Connecting%20to%20Services.md)
### What credentials were found for the local database instance while debugging the octopus_checker binary? (Format username:password)

```shell
./octopus_checker

gdb ./octopus_checker
```
![](Screenshot%202026-08-03%20at%2012.33.23.png)

```assembly
b *0x5555555551b0

run
```
![](Screenshot%202026-08-03%20at%2012.37.17.png)