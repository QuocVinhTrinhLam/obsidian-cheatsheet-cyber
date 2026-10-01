
![](Hashcat%20Rules-20260923-090140.png)
## What Are Hashcat Rules?

Rules are sets of instructions applied to each `word` in a `wordlist`, generating variations through transformations like character substitutions, case changes, reversals, and symbol appending. By mimicking the kinds of modifications users commonly make to their passwords, these rules significantly expand our coverage and boost the chances of a successful crack.

For example, rules have the ability to:

- Append/prepend characters (e.g., `Password` → `Password1`)
- Replace characters (`a` → `@`)
- Capitalize letters (`password` → `PASSWORD`)
- Reverse words (`password` → `drowssap`)
#### Hashcat Example Rule Operations

|**Rule**|**Description**|**Example**|
|---|---|---|
|`c`|Capitalize the first character, lowercase the rest|password → Password|
|`C`|Lowercase the first character, uppercase the rest|password → pASSWORD|
|`t`|Toggle the case of all characters in word|password → PASSWORD|
|`T2`|Toggle the case of characters at position 3|password → paSsword|
|`$1`|Append 1 to the end|password → password1|
|`^1`|Prepend 1 to the front|password → 1password|
|`r`|Reverse the word|password → drowssap|
|`sa@`|Substitute a with @|password → p@ssword|
|`d`|Duplicate the word|password → passwordpassword|
|`z5`|Duplicate first character 5 times|password → ppppppassword|
|`Z5`|Duplicate last character 5 times|password → passwordddddd|
### Creating Hashcat Rule

Let's consider a scenario where we want to create a Hashcat rule to convert the word `winteriscoming` into `W1nt3r1sc0m1ng!`. This transformation involves several steps.

- Capitalizing the first letter (`w → W`)
- Replacing specific characters: (`i → 1, e → 3, o → 0`)
- Appending an exclamation mark (`!`) at the end

```sh
3kjS@htb[/htb]$ cat hashcatrule

T0si1se3so0$!
```

Here is a quick breakdown of the rule's syntax:

- `T0` — Capitalize the character at position 0 (the first letter)
- `si1` — Substitute all instances of i with 1
- `se3` — Substitute all instances of e with 3
- `so0` — Substitute all instances of o with 0
- `$!` — Append ! to the end of the word

```sh
3kjS@htb[/htb]$ cat wordlist

winteriscoming
```

```sh
3kjS@htb[/htb]$ cat hashcatrule

T0si1se3so0$!
```

```sh
3kjS@htb[/htb]$ hashcat -r hashcatrule wordlist --stdout

W1nt3r1sc0m1ng!
```
### Using Hashcat Rules

```sh
3kjS@htb[/htb]$ ls -l /usr/share/hashcat/rules/
```
![](Screenshot%202026-09-23%20at%2009.06.58.png)

To apply a rule file to your wordlist in Hashcat, use the `-r` option followed by the path to the rule file. For example, to use `T0XlC.rule`, we would run the command like this:

```sh
3kjS@htb[/htb]$ hashcat -m 22000 wpa_hash wordlist.txt -r /usr/share/hashcat/rules/T0XlC.rule
```

There are a variety of publicly available, well-known rules and collections, such as:

|**Rule**|**Description**|**Link**|
|---|---|---|
|`one-rule-to-rule-them-all`|One super rule that tops all the tests|[Link](https://github.com/stealthsploit/OneRuleToRuleThemStill/blob/main/OneRuleToRuleThemStill.rule)|
|`rules-collection`|Largest collection of hashcat rule-files|[Link](https://github.com/n0kovo/hashcat-rules-collection)|
|`nsa-rules`|Rules generated from cracked passwords|[Link](https://github.com/NSAKEY/nsa-rules)|
|`Hob0Rules`|Rule based on statistics and industry patterns|[Link](https://github.com/praetorian-inc/Hob0Rules)|
