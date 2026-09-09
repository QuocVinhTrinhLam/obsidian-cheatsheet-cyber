# [Recursive Fuzzing](CWES/Web%20Fuzzing/Recursive%20Fuzzing.md)
### Recursively fuzz the "recursive_fuzz" path on the target system (ie http://IP:PORT/recursive_fuzz/) to find the flag.

```shell
ffuf -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -ic -u http://154.57.164.82:32428/ recursive_fuzz/level1/level2/level3/FUZZ -e .html -recursion -recursion-depth 2 -rate 500
```
![](Screenshot%202026-09-08%20at%2015.43.07.png)
![](Screenshot%202026-09-08%20at%2015.39.25.png)