## Why Fuzz APIs?

API fuzzing is crucial for several reasons:

- `Uncovering Hidden Vulnerabilities`: APIs often have hidden or undocumented endpoints and parameters that can be susceptible to attacks. Fuzzing helps uncover these hidden attack surfaces.
- `Testing Robustness`: Fuzzing assesses the API's ability to gracefully handle unexpected or malformed input, ensuring it doesn't crash or expose sensitive data.
- `Automating Security Testing`: Manual testing of all possible input combinations is infeasible. Fuzzing automates this process, saving time and effort.
- `Simulating Real-World Attacks`: Fuzzing can mimic the actions of malicious actors, allowing you to identify vulnerabilities before attackers exploit them.
## Types of API Fuzzing

There are 3 primary types of API fuzzing

1. `Parameter Fuzzing` - One of the primary techniques in API fuzzing, parameter fuzzing focuses on systematically testing different values for API parameters. This includes query parameters (appended to the API endpoint URL), headers (containing metadata about the request), and request bodies (carrying the data payload). By injecting unexpected or invalid values into these parameters, fuzzers can expose vulnerabilities like injection attacks (e.g., SQL injection, command injection), cross-site scripting (XSS), and parameter tampering.
2. `Data Format Fuzzing` - Web APIs frequently exchange data in structured formats like JSON or XML. Data format fuzzing specifically targets these formats by manipulating the structure, content, or encoding of the data. This can reveal vulnerabilities related to parsing errors, buffer overflows, or improper handling of special characters.
3. `Sequence Fuzzing` - APIs often involve multiple interconnected endpoints, where the order and timing of requests are crucial. Sequence fuzzing examines how an API responds to sequences of requests, uncovering vulnerabilities like race conditions, insecure direct object references (IDOR), or authorization bypasses. By manipulating the order, timing, or parameters of API calls, fuzzers can expose weaknesses in the API's logic and state management.
## Exploring the API

![](API%20Fuzzing-20260909-140905.png)

The specification details five endpoints, each with a specific purpose and method:

1. `GET /` (Read Root): This fetches the root resource. It likely returns a basic welcome message or API information.
2. `GET /items/{item_id}` (Read Item): Retrieves a specific item identified by `item_id`.
3. `DELETE /items/{item_id}` (Delete Item): Deletes an item identified by `item_id`.
4. `PUT /items/{item_id}` (Update Item): Updates an existing item with the provided data.
5. `POST /items/` (Create Or Update Item): This function creates a new item or updates an existing one if the `item_id` matches.
## Fuzzing the API

```sh
3kjS@htb[/htb]$ git clone https://github.com/PandaSt0rm/webfuzz_api.git
3kjS@htb[/htb]$ cd webfuzz_api
3kjS@htb[/htb]$ pip3 install -r requirements.txt
```

```sh
3kjS@htb[/htb]$ python3 api_fuzzer.py http://IP:PORT

[-] Invalid endpoint: http://localhost:8000/~webmaster (Status code: 404)
[-] Invalid endpoint: http://localhost:8000/~www (Status code: 404)

Fuzzing completed.
Total requests: 4730
Failed requests: 0
Retries: 0
Status code counts:
404: 4727
200: 2
405: 1
Found valid endpoints:
- http://localhost:8000/cz...
- http://localhost:8000/docs
Unusual status codes:
405: http://localhost:8000/items
```

We can explore the undocumented endpoint via curl and it will return a flag:

```sh
3kjS@htb[/htb]$ curl http://localhost:8000/cz... 

{"flag":"<snip>"}
```
