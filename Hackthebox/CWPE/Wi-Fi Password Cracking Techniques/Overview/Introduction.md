## Wi-Fi Encryption Standards

Wireless networks typically use one of these security protocols:

| **Protocol**                              | **Status**  | **Security Level**                         |
| ----------------------------------------- | ----------- | ------------------------------------------ |
| `802.11 (Legacy)`                         | Deprecated  | Very Weak                                  |
| `802.11b (WEP)`                           | Deprecated  | Very Weak                                  |
| `802.11g/n (WPA)`                         | Deprecated  | Weak                                       |
| `802.11n/ac (WPA2)`                       | Widely used | Strong (if passphrase is strong)           |
| `802.11ac/ax (WPA3)`                      | Current     | Strongest (so far)                         |
| `Open Networks`                           | Active      | No Security                                |
| `OWE (Opportunistic Wireless Encryption)` | Emerging    | Better than open, but lacks authentication |
| `WPA2-Enterprise`                         | Active      | High (with certificate validation)         |
| `WPA3-Enterprise`                         | Active      | Very High                                  |
## The Traditional WPA Password Attack

A traditional WPA password attack involves four key steps: Reconnaissance, Handshake Capture, Password Cracking, and Access Verification.

1. `Reconnaissance`: Identify nearby Wi-Fi networks (airodump-ng).
2. `Handshake Capture`: Listen for or trigger a handshake by disconnecting a connected client (aireplay-ng).
3. `Password Cracking`: Use a wordlist or brute-force to attempt to crack the handshake (aircrack-ng, hashcat, cowpatty, john).
4. `Access Verification`: If cracked successfully, test the key by connecting to the network.

In the next section, we'll take a hands-on approach and explore a variety of tools commonly used to crack WPA/WPA2 passwords.