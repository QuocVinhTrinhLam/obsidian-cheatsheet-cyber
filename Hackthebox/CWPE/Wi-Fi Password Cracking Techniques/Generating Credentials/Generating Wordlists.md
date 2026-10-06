### Generating Personalized Wordlists
#### Using CUPP

```sh
cupp [-h] [-i | -w FILENAME | -l | -a | -v] [-q]
```

- `-h`: Shows the help message and exit
- `-i`: Interactive questions for user password profiling
- `-w`: Use this option to improve existing dictionary, or WyD.pl output to make some pwnsauce
- `-l`: Download huge wordlists from repository
- `-a`: Parse default usernames and passwords directly from Alecto DB.
- `-v`: Shows the version of this program.
- `-q`: Runs cupp in quiet mode

```sh
3kjS@htb[/htb]$ cupp -i
```
### Generating Wordlists from a Website
#### Using CeWL

```sh
cewl -d <depth> -m <min_word_length> -w <output_file> <target_url>
```

- `-d`: Sets how deep CeWL should spider through the website’s links.
- `-m`: Sets the minimum length of the words to include (useful to align with password policies).
- `-w`: Defines the output file where the generated wordlist will be saved.
- `<target_url>`: The URL of the website to scan.

```sh
3kjS@htb[/htb]$ cewl http://logistics.local -d 4 -m 8 --lowercase -w inlane.wordlist

CeWL 5.5.2 (Grouping) Robin Wood (robin@digi.ninja) (https://digi.ninja/)
```
### Generating Wordlists based on Parameters
#### Using Crunch

```sh
crunch <min_length> <max_length> <charset> -t <pattern> -o <output_file>
```

- `<min_length>` and `<max_length>`: Defines the minimum and maximum length of the generated passwords.
- `<charset>`: Defines which characters to include (e.g., lowercase, uppercase, digits, symbols).
- `-t <pattern>`: Specifies a pattern to follow when generating passwords.
- `-o <output_file>`: Saves the output to a specified file.

Pattern Placeholders (Used with -t option):

- `@` → Lowercase letters (a–z)
- `,` → Uppercase letters (A–Z)
- `%` → Numbers (0–9)
- `^` → Symbols (e.g., !, @, #)

```sh
3kjS@htb[/htb]$ crunch 8 8 0123456789 -t Ab%%%%%% -o number_passwords.txt
```
