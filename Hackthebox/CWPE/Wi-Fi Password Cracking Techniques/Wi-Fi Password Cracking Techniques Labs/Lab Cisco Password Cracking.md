# [Cisco Password Cracking](Cisco%20Password%20Cracking.md)
### Use the Cisco configuration file located at "/opt/CISCO/cisco.conf" to crack the password for the user jsomeone. What is the password?

![](Screenshot%202026-10-06%20at%2008.59.21.png)
### Use the Cisco configuration file located at "/opt/CISCO/cisco.conf" to crack the password for the user monty. What is the password?

![](Screenshot%202026-10-06%20at%2009.00.59.png)

```sh
hashcat -m 5700 -a 0 KyOkk8sHvpcUtuKVQfybAyLtbZwczG29bapi.LasfCE /usr/share/wordlists/rockyou.txt
```

![](Screenshot%202026-10-06%20at%2009.05.21.png)
### Use the Cisco configuration file located at "/opt/CISCO/cisco.conf" to crack the password for the user jerry. What is the password?

![](Screenshot%202026-10-06%20at%2009.00.59.png)

```
wget https://raw.githubusercontent.com/theevilbit/ciscot7/master/ciscot7.py
```

```sh
python3 ciscot7.py -d -p 0538550C331F5A39391604
Decrypted password: S3cr3tP@ss
```
### Use the Cisco configuration file located at "/opt/CISCO/cisco.conf" to crack the password for the user bob. What is the password?

![](Screenshot%202026-10-06%20at%2009.00.59.png)

```sh
echo '$8$riEI/v2zPOfQLy$Z/Ic9wyCz.Cd3qc73NiLUFeomBHvYYAB2J2msERV8Qk' > hash
```

```sh
sudo hashcat -m 9200 -a 0 hash /usr/share/wordlists/rockyou.txt
```

![](Screenshot%202026-10-06%20at%2009.09.22.png)
