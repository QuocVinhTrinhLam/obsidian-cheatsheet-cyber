# [Airdecap-ng](Airdecap-ng.md)
### Decrypt the file located at /opt/decrypt.cap using airdecap-ng. Look for sensitive data indicating a user is attempting to log in to a website with a POST request. What is the username associated with this login attempt? (The WPA key for ESSID named CyberNet-Secure is Password123!!!!!!)

```sh
sudo airdecap-ng -p 'Password123!!!!!!' -e 'CyberNet-Secure' decrypt.cap
```
![](Screenshot%202026-09-16%20at%2014.51.43.png)

```wireshark
http.request.method == "POST"
```
![](Screenshot%202026-09-16%20at%2014.54.02.png)
### Decrypt the file located at /opt/decrypt.cap using airdecap-ng. Look for sensitive data indicating a user is attempting to log in to a website with a POST request. What is the password entered during this login attempt? (The WPA key for ESSID named CyberNet-Secure is Password123!!!!!!)

![](Screenshot%202026-09-16%20at%2014.54.02.png)