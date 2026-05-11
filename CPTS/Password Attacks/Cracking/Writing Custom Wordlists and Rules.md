|**Description**|**Password Syntax**|
|---|---|
|First letter is uppercase|`Password`|
|Adding numbers|`Password123`|
|Adding year|`Password2022`|
|Adding month|`Password02`|
|Last character is an exclamation mark|`Password2022!`|
|Adding special characters|`P@ssw0rd2022!`|

```shell
3kjS@htb[/htb]$ cat password.list password
```

|**Function**|**Description**|
|---|---|
|`:`|Do nothing|
|`l`|Lowercase all letters|
|`u`|Uppercase all letters|
|`c`|Capitalize the first letter and lowercase others|
|`sXY`|Replace all instances of X with Y|
|`$!`|Add the exclamation character at the end|

```shell
3kjS@htb[/htb]$ cat custom.rule

: 
c 
so0 
c 
so0 
sa@ 
c 
sa@ 
c 
sa@ 
so0
...
```

We can use the following command to apply the rules in `custom.rule` to each word in `password.list` and store the mutated results in `mut_password.list`.

```shell
3kjS@htb[/htb]$ hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
```

In this case, the single input word will produce fifteen mutated variants.

```shell
3kjS@htb[/htb]$ cat mut_password.list 
password 
Password 
passw0rd 
Passw0rd 
p@ssword 
P@ssword 
P@ssw0rd 
password! 
Password! 
passw0rd! 
...
```
## Generating wordlists using CeWL

```shell
3kjS@htb[/htb]$ cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist 
3kjS@htb[/htb]$ wc -l inlane.wordlist 

326
```
