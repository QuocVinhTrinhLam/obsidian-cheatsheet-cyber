## The Perils of Default Credentials
|Device/Manufacturer|Default Username|Default Password|Device Type|
|---|---|---|---|
|Linksys Router|admin|admin|Wireless Router|
|D-Link Router|admin|admin|Wireless Router|
|Netgear Router|admin|password|Wireless Router|
|TP-Link Router|admin|admin|Wireless Router|
|Cisco Router|cisco|cisco|Network Router|
|Asus Router|admin|admin|Wireless Router|
|Belkin Router|admin|password|Wireless Router|
|Zyxel Router|admin|1234|Wireless Router|
|Samsung SmartCam|admin|4321|IP Camera|
|Hikvision DVR|admin|12345|Digital Video Recorder (DVR)|
|Axis IP Camera|root|pass|IP Camera|
|Ubiquiti UniFi AP|ubnt|ubnt|Wireless Access Point|
|Canon Printer|admin|admin|Network Printer|
|Honeywell Thermostat|admin|1234|Smart Thermostat|
|Panasonic DVR|admin|12345|Digital Video Recorder (DVR)|
### Brute-forcing and Password Security

For a pentester, this translates to a deeper understanding of the target's security posture:

- `Evaluating System Vulnerability:` Password policies, or their absence, and the likelihood of users employing weak passwords directly inform the potential success of a brute-force attack.
- `Strategic Tool Selection:` The complexity of the passwords dictates the tools and methodologies a pentester will deploy. A simple dictionary attack might suffice for weak passwords, while a more sophisticated, hybrid approach may be required to crack stronger ones.
- `Resource Allocation:` The estimated time and computational power needed for a brute-force attack is intrinsically linked to the complexity of the passwords. This knowledge is essential for effective planning and resource management.
- `Exploiting Weak Points:` Default passwords are often a system's Achilles' heel. A pentester's ability to identify and leverage these easily guessable credentials can provide a swift entry point into the target network.