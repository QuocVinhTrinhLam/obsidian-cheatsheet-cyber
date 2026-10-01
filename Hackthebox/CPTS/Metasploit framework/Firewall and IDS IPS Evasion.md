| **Security Policy**                         | **Description**                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Signature-based Detection`                 | The operation of packets in the network and comparison with pre-built and pre-ordained attack patterns known as signatures. Any 100% match against these signatures will generate alarms.                                                                                                                                         |
| `Heuristic / Statistical Anomaly Detection` | Behavioral comparison against an established baseline included modus-operandi signatures for known APTs (Advanced Persistent Threats). The baseline will identify the norm for the network and what protocols are commonly used. Any deviation from the maximum threshold will generate alarms.                                   |
| `Stateful Protocol Analysis Detection`      | Recognizing the divergence of protocols stated by event comparison using pre-built profiles of generally accepted definitions of non-malicious activity.                                                                                                                                                                          |
| `Live-monitoring and Alerting (SOC-based)`  | A team of analysts in a dedicated, in-house, or leased SOC (Security Operations Center) use live-feed software to monitor network activity and intermediate alarming systems for any potential threats, either deciding themselves if the threat should be actioned upon or letting the automated mechanisms take action instead. |
#### VirusTotal

```shell
3kjS@htb[/htb]$ msf-virustotal -k <API key> -f test.js
```
#### Archiving the Payload

```shell
3kjS@htb[/htb]$ wget https://www.rarlab.com/rar/rarlinux-x64-612.tar.gz 

3kjS@htb[/htb]$ tar -xzvf rarlinux-x64-612.tar.gz && cd rar 

3kjS@htb[/htb]$ rar a ~/test.rar -p ~/test.js
```
#### Removing the .RAR Extension

```shell
3kjS@htb[/htb]$ mv test.rar test 

3kjS@htb[/htb]$ ls
```
#### Archiving the Payload Again

```shell
3kjS@htb[/htb]$ rar a test2.rar -p test
```
#### Removing the .RAR Extension

```shell
3kjS@htb[/htb]$ mv test2.rar test2 

3kjS@htb[/htb]$ ls
```

