# [Airgraph-ng](Airgraph-ng.md)
### Use airgraph-ng on the file /opt/data.csv to create a graph of Clients to AP Relationship (CAPR). How many total clients are shown in the generated graphic? (Answer in digit format: e.g., 3)

9

```sh
sudo airgraph-ng -i data.csv -g CPG -o lab.png
```
![](Screenshot%202026-09-16%20at%2014.17.55.png)
### Use airgraph-ng on the file /opt/data.csv to create a graph of Clients to AP Relationship (CAPR). How many clients are connected to the AP 'CyberNet-Secure'? (Answer in digit format: e.g., 3)

6

![](Screenshot%202026-09-16%20at%2014.17.55.png)
### Use airgraph-ng on the file /opt/data.csv to create a Common Probe graph (CPG). How many clients are probing for the AP 'HTB-Wireless'? (Answer in digit format: e.g., 3)

2

![](Screenshot%202026-09-16%20at%2014.17.55.png)