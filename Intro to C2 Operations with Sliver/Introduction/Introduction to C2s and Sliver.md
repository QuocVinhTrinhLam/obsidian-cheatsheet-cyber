A command and control (C2) server is software tasked to execute commands or binaries on a remote computer, or a network of computers. The primary focus of a C2 is to have a centralized management system where the operator can manage access to other machines somewhere in the network. An operator is the one who carries out the (simulated) attack and manages the software. The access can be gained in multiple ways, be it a SQL Injection vulnerability, weak credentials on different services such as SSH, RDP, or access to the initial machine (foothold) given by the client for a red team engagement or a penetration test. The term "red team" refers to a group of people specializing in researching different ways getting into systems and refining tools, and being a step ahead of defenders.
## Attack Lifecycle
|Attack Lifecycle|Description|
|---|---|
|Reconnaissance|starts with gathering as much information as possible about the target. It can be active reconnaissance (actively interacting with the target) or passive reconnaissance. Such recon can include active scanning, gathering information about the victim's hosts/identity or the network, and searching through the open and deep web.|
|Weaponization|characterizing the process of the development of the payload allowing foothold access.|
|Delivery|constitutes a stage when one has found a way of transferring the payload onto the target.|
|Exploitation|the step where one executes the payload onto the target.|
|Installation|the step during which the adversary establishes initial control over the target|
|Command and Control (C2)|constitutes a step wherein one has established a connection from the target to the command and control server.|
|Actions on Objectives|the step where one starts carrying out the intended goals, whether data theft or exfiltration.|
## Sliver

[Sliver](https://github.com/BishopFox/sliver) is a command and control software developed by [BishopFox](https://bishopfox.com/). Used by penetration testers and red teamers, its client, server, and beacons (known as implants) are written in [Golang](https://go.dev/) - making it easy to cross-compile for different platforms.
