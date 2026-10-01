### Clients to AP Relationship Graph

The access points are color-coded based on their encryption type:

- Green for WPA
- Yellow for WEP
- Red for open networks
- Black for unknown encryption.

```sh
3kjS@htb[/htb]$ sudo airgraph-ng -i HTB-01.csv -g CAPR -o HTB_CAPR.png
```
![](Airgraph-ng-20260916-141346.png)
### Common Probe Graph

```sh
3kjS@htb[/htb]$ sudo airgraph-ng -i HTB-01.csv -g CPG -o HTB_CPG.png
```
![](Airgraph-ng-20260916-141427.png)
