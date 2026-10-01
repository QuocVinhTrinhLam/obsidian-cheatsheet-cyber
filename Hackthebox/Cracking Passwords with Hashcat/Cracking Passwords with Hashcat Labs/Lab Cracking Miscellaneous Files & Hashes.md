# [Cracking Miscellaneous Files & Hashes](Cracking%20Miscellaneous%20Files%20&%20Hashes.md)
### Extract the hash from the attached 7-Zip file, crack the hash, and submit the value of the flag.txt file contained inside the archive.

![](Screenshot%202026-09-29%20at%2016.56.13.png)

```sh
hashcat --username -a 0 -m 11600 zipfile.hash /usr/share/wordlists/rockyou.txt
```

![](Screenshot%202026-09-29%20at%2017.00.17.png)