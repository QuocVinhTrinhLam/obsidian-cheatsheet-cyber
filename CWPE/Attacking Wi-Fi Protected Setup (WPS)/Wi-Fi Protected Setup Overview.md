### WPS Connection Methods

There are four methods to connect to a WPS-enabled access point. Each of them is detailed below:

| Method                            | Description                                                                                                                                                                                                                                                |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Push Button Configuration (PBC)` | This is the most common method and involves pressing a physical or virtual button on the router and the client device. Once the button is pressed on both devices, they automatically exchange the necessary information to establish a secure connection. |
| `PIN Entry`                       | Each WPS-enabled device has an 8-digit PIN code, either provided by the manufacturer or displayed on the device. Users enter this PIN on their router or access point’s configuration page to connect the device to the network.                           |
| `Near-field communication method` | Some devices support NFC, allowing users to tap the device on the router to establish a connection. This method is less common but offers an additional level of convenience.                                                                              |
| `USB Flash Drive`                 | Involves transferring configuration settings via a USB drive from the router to the client device. This method is rarely used due to the inconvenience compared to other methods.                                                                          |
### Benefits of WPS

- `Ease of Use`: Simplifies the process of adding new devices to a wireless network, making it accessible even for non-technical users.
- `Convenience`: Eliminates the need to remember or enter long and complex passwords.

---

### Security Concerns

While WPS was designed to make network connections simpler, it has notable security vulnerabilities:

- `PIN Method Vulnerability`: The 8-digit PIN can be cracked relatively easily through brute-force attacks due to the way the protocol verifies the PIN in two halves.
- `Physical Security Risks`: The PBC method relies on physical security, meaning an unauthorized person within range could potentially push the button and connect to the network.

Wi-Fi Protected Setup provides an easy way to connect devices to a Wi-Fi network, but it comes with significant security risks, especially with the PIN method. Understanding these risks and taking steps to mitigate them, such as disabling WPS and using robust security protocols, can help protect your network.