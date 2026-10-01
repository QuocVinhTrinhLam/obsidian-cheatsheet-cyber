## Creating Wordlists
## Crunch
#### Crunch - Syntax

```sh
3kjS@htb[/htb]$ crunch <minimum length> <maximum length> <charset> -t <pattern> -o <output file>
```
#### Crunch - Generate Word List

```sh
3kjS@htb[/htb]$ crunch 4 8 -o wordlist
```
#### Crunch - Create Word List using Pattern

```sh
3kjS@htb[/htb]$ crunch 17 17 -t ILFREIGHT201%@@@@ -o wordlist
```
#### Crunch - Specified Repetition

```sh
3kjS@htb[/htb]$ crunch 12 12 -t 10031998@@@@ -d 1 -o wordlist
```
## CUPP
#### CUPP - Usage Example

```sh
3kjS@htb[/htb]$ python3 cupp.py -i

[+] Insert the information about the victim to make a dictionary
[+] If you don't know all the info, just hit enter when asked! ;)

> First Name: roger
> Surname: penrose
> Nickname:      
> Birthdate (DDMMYYYY): 11051972

> Partners) name: beth
> Partners) nickname:
> Partners) birthdate (DDMMYYYY):

> Child's name: john
> Child's nickname: johnny
> Child's birthdate (DDMMYYYY):

> Pet's name: tommy
> Company name: INLANE FREIGHT

> Do you want to add some key words about the victim? Y/[N]: Y
> Please enter the words, separated by comma. [i.e. hacker,juice,black], spaces will be removed: sysadmin,linux,86391512
> Do you want to add special chars at the end of words? Y/[N]:
> Do you want to add some random numbers at the end of words? Y/[N]:
> Leet mode? (i.e. leet = 1337) Y/[N]:

[+] Now making a dictionary...
[+] Sorting list and removing duplicates...
[+] Saving dictionary to roger.txt, counting 2419 words.
[+] Now load your pistolero with roger.txt and shoot! Good luck!
```
## KWPROCESSOR
#### Kwprocessor - Installation

```sh
3kjS@htb[/htb]$ git clone https://github.com/hashcat/kwprocessor
3kjS@htb[/htb]$ cd kwprocessor
3kjS@htb[/htb]$ make
```
#### Kwprocessor - Example

```sh
3kjS@htb[/htb]$ kwp -s 1 basechars/full.base keymaps/en-us.keymap  routes/2-to-10-max-3-direction-changes.route
```
## Princeprocessor
#### Wordlist

```txt
dog
cat
ball
```
#### Princeprocessor - Generated Wordlist

```txt
balldog
ballcat
ballball
dogdogdog
catdogdog
dogcatdog
catcatdog
dogdogcat
<SNIP>
```
#### Princeprocessor - Installation

```sh
3kjS@htb[/htb]$ wget https://github.com/hashcat/princeprocessor/releases/download/v0.22/princeprocessor-0.22.7z
3kjS@htb[/htb]$ 7z x princeprocessor-0.22.7z
3kjS@htb[/htb]$ cd princeprocessor-0.22
3kjS@htb[/htb]$ ./pp64.bin -h
```
#### Princeprocessor - Find the Number of Combinations

```sh
3kjS@htb[/htb]$ ./pp64.bin --keyspace < words

232
```
#### Princeprocessor - Forming Wordlist

```sh
3kjS@htb[/htb]$ ./pp64.bin -o wordlist.txt < words
```
#### Princeprocessor - Password Length Limits

```sh
3kjS@htb[/htb]$ ./pp64.bin --pw-min=10 --pw-max=25 -o wordlist.txt < words
```
#### Princeprocessor - Specifying Elements

```sh
3kjS@htb[/htb]$ ./pp64.bin --elem-cnt-min=3 -o wordlist.txt < words
```
## CeWL
#### CeWL - Syntax

```sh
3kjS@htb[/htb]$ cewl -d <depth to spider> -m <minimum word length> -w <output wordlist> <url of website>
```
#### CeWL - Example

```sh
3kjS@htb[/htb]$ cewl -d 5 -m 8 -e http://inlanefreight.com/blog -w wordlist.txt
```
## Previously Cracked Passwords

```sh
3kjS@htb[/htb]$ cut -d: -f 2- ~/hashcat.potfile
```
## Hashcat-utils

```sh
3kjS@htb[/htb]$ /mp64.bin Welcome?s
Welcome 
Welcome!
Welcome"
Welcome#
Welcome$
Welcome%
Welcome&
Welcome'
Welcome(
Welcome)
Welcome*
Welcome+

<SNIP>
```
