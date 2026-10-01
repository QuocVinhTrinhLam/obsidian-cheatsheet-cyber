|**Function**|**Description**|**Input**|**Output**|
|---|---|---|---|
|l|Convert all letters to lowercase|InlaneFreight2020|inlanefreight2020|
|u|Convert all letters to uppercase|InlaneFreight2020|INLANEFREIGHT2020|
|c / C|Capitalize first character, lowercase the rest / Lowercase first character, uppercase the rest|inlaneFreight2020 / Inlanefreight2020|Inlanefreight2020 / iNLANEFREIGHT2020|
|t / TN|Toggle case : whole word / at position N|InlaneFreight2020|iNLANEfREIGHT2020|
|d / q / zN / ZN|Duplicate word / all characters / first character / last character|InlaneFreight2020|InlaneFreight2020InlaneFreight2020 / IInnllaanneeFFrreeiigghhtt22002200 / IInlaneFreight2020 / InlaneFreight20200|
|{ / }|Rotate word left / right|InlaneFreight2020|nlaneFreight2020I / 0InlaneFreight202|
|^X / $X|Prepend / Append character X|InlaneFreight2020 (^! / $! )|!InlaneFreight2020 / InlaneFreight2020!|
|r|Reverse|InlaneFreight2020|0202thgierFenalnI|
## Example Rule Creation
#### Rules

```sh
c so0 si1 se3 ss5 sa@ $2 $0 $1 $9
```
#### Create a Rule File

```sh
3kjS@htb[/htb]$ echo 'c so0 si1 se3 ss5 sa@ $2 $0 $1 $9' > rule.txt
```
#### Store the Password in a File

```sh
3kjS@htb[/htb]$ echo 'password_ilfreight' > test.txt
```
#### Hashcat - Debugging Rules

```sh
3kjS@htb[/htb]$ hashcat -r rule.txt test.txt --stdout

P@55w0rd_1lfr31ght2019
```
#### Generate SHA1 Hash

```sh
3kjS@htb[/htb]$ echo -n 'St@r5h1p2019' | sha1sum | awk '{print $1}' | tee hash
```
#### Hashcat - Cracking Passwords Using Wordlists and Rules

```sh
3kjS@htb[/htb]$ hashcat -a 0 -m 100 hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt -r rule.txt
```
#### Hashcat - Default Rules

```sh
3kjS@htb[/htb]$ ls -l /usr/share/hashcat/rules/
```
![](Screenshot%202026-09-29%20at%2016.10.22.png)
#### Hashcat - Generate Random Rules

```sh
3kjS@htb[/htb]$ hashcat -a 0 -m 100 -g 1000 hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```
