## TTPs
#### Tcpdump Output

```shell
3kjS@htb[/htb]$ sudo tcpdump -i ens224
```
#### Starting Responder

```shell
sudo responder -I ens224 -A
```
#### FPing Active Checks

```shell
3kjS@htb[/htb]$ fping -asgq 172.16.5.0/23
```
#### Nmap Scanning

```shell
sudo nmap -v -A -iL hosts.txt -oN /home/htb-student/Documents/host-enum
```
## Identifying Users
### Kerbrute - Internal AD Username Enumeration

We will use Kerbrute in conjunction with the `jsmith.txt` or `jsmith2.txt` user lists from [Insidetrust](https://github.com/insidetrust/statistically-likely-usernames).
#### Cloning Kerbrute GitHub Repo

```shell
3kjS@htb[/htb]$ sudo git clone https://github.com/ropnop/kerbrute.git
```
#### Compiling for Multiple Platforms and Architectures

```shell
3kjS@htb[/htb]$ sudo make all
```
#### Listing the Compiled Binaries in dist

```shell
3kjS@htb[/htb]$ ls dist/ 

kerbrute_darwin_amd64 kerbrute_linux_386 kerbrute_linux_amd64 
kerbrute_windows_386.exe kerbrute_windows_amd64.exe
```
#### Testing the kerbrute_linux_amd64 Binary

```shell
3kjS@htb[/htb]$ ./kerbrute_linux_amd64
```
#### Adding the Tool to our Path

```shell
3kjS@htb[/htb]$ echo $PATH /home/htb-student/.local/bin:/snap/bin:/usr/sandbox/:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/usr/share/games:/usr/local/sbin:/usr/sbin:/sbin:/snap/bin:/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/home/htb-student/.dotnet/tools
```
#### Moving the Binary

```shell
3kjS@htb[/htb]$ sudo mv kerbrute_linux_amd64 /usr/local/bin/kerbrute
```
#### Enumerating Users with Kerbrute

```shell
3kjS@htb[/htb]$ kerbrute userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o valid_ad_users
```
