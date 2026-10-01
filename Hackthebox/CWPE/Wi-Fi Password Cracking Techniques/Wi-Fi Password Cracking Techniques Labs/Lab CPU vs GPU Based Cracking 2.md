# [CPU vs GPU Based Cracking 2](CPU%20vs%20GPU%20Based%20Cracking%202.md)
#### Exercise

Crack the following WPA2 hash using the [top-1000](https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/Pwdb_top-1000.txt) wordlist using both CPU and GPU simultaneously.

```hash
WPA*02*5c9b6f3a0f364903bb19dc76f3e03a9b*020000000800*020000000900*485442*a69f727ad36d6f6b205d64080932da1f9b5630492ba20f233d05477086b04b6e*0103007502010a00000000000000000001613009d242bfff28e249291f784ec8d1cdbf4e2682fd7afadf493f2a87d3ef9f000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001630140100000fac040100000fac040100000fac020000*80
```

Then, practice cracking the password while using CPU mode and GPU mode separately. Take note of the time difference between the two methods to better understand their performance impact.
### What is the cracked password?

```sh
hashcat -m 22000 hash Pwdb_top-1000.txt -D 1,2 -d 1,2 --cpu-affinity=1,2,3,4 --hook-threads=8
```

![](Screenshot%202026-09-23%20at%2009.00.34.png)

