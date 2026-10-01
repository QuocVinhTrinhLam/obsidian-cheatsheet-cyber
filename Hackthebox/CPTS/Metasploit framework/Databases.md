## Setting up the Database
#### PostgreSQL Status

```shell
3kjS@htb[/htb]$ sudo service postgresql status
```
#### Start PostgreSQL

```shell
3kjS@htb[/htb]$ sudo systemctl start postgresql
```
#### MSF - Initiate a Database

```shell
3kjS@htb[/htb]$ sudo msfdb init
```

Sometimes an error can occur if Metasploit is not up to date.First, often it helps to update Metasploit again (`apt update`) to solve this problem.

```shell
3kjS@htb[/htb]$ sudo msfdb init
```

```shell
3kjS@htb[/htb]$ sudo msfdb status
```

If this error does not appear, which often happens after a fresh installation of Metasploit, then we will see the following when initializing the database:

```shell
3kjS@htb[/htb]$ sudo msfdb init
```
#### MSF - Connect to the Initiated Database

```shell
3kjS@htb[/htb]$ sudo msfdb run
```
#### MSF - Reinitiate the Database

```shell
3kjS@htb[/htb]$ msfdb reinit 
3kjS@htb[/htb]$ cp /usr/share/metasploit-framework/config/database.yml ~/.msf4/ 
3kjS@htb[/htb]$ sudo service postgresql restart 
3kjS@htb[/htb]$ msfconsole -q
```
#### MSF - Database Options

```shell
msf6 > help database
```
## Using the Database

```shell
msf6 > workspace
```

```shell
msf6 > workspace -a Target_1 

[*] Added workspace: Target_1 
[*] Workspace: Target_1 

msf6 > workspace Target_1 

[*] Workspace: Target_1 msf6 > workspace 

  default 
* Target_1
```
## Importing Scan Results
#### Stored Nmap Scan

```shell
3kjS@htb[/htb]$ cat Target.nmap
```
#### Importing Scan Results

```shell
msf6 > db_import Target.xml
```
## Using Nmap Inside MSFconsole
#### MSF - Nmap

```shell
msf6 > db_nmap -sV -sS 10.10.10.8

msf6 > hosts

msf6 > services
```
## Credentials
#### MSF - Stored Credentials

```shell
msf6 > creds -h
```
