## 1. CPU-Based Cracking

```sh
3kjS@htb[/htb]$ hashcat -m 22000 hash /opt/wordlist.txt -D 1
```

We can further bind hashcat to specific CPU cores using the `--cpu-affinity` flag. This allows finer control over which cores handle the cracking task.

```sh
3kjS@htb[/htb]$ hashcat -m 22000 hash /opt/wordlist.txt -D 1 --cpu-affinity=1,2,3,4
```

To control the number of threads hashcat uses, we can use the `--hook-threads` option.

```sh
3kjS@htb[/htb]$ hashcat -m 22000 hash /opt/wordlist.txt -D 1 --cpu-affinity=1,2,3,4 --hook-threads=8
```

- `--cpu-affinity`: Bind specific CPU cores for optimized performance
- `--hook-threads`: Specify the number of CPU threads Hashcat can hook into
## 2. GPU-Based Cracking

```sh
3kjS@htb[/htb]$ hashcat -m 22000 hash /opt/wordlist.txt --optimized-kernel-enable -w 4
```
## 3. Using CPU and GPU Simultaneously

```sh
3kjS@htb[/htb]$ hashcat -m 22000 hash /opt/wordlist.txt -D 1,2 -d 1,2 --cpu-affinity=1,2,3,4 --hook-threads=8
```

- `-D 1,2`: Use both CPU (1) and GPU (2) device types
- `-d 1,2`: Use specific backend device IDs (e.g., CPU ID 1 and GPU ID 2)

Crack the following WPA2 hash using the [top-1000](https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/Pwdb_top-1000.txt) wordlist using both CPU and GPU simultaneously.
