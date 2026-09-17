There are two primary authentication systems commonly used in WiFi networks: `Open System Authentication` and `Shared Key Authentication`.

![](Authentication%20Methods-20260916-102610.png)

- `Open System Authentication` is straightforward and does not require any shared secret or credentials for initial access. This type of authentication is typically used in open networks where no password is needed, allowing any device to connect to the network without prior verification.
- `Shared Key Authentication`, as the name suggests, involves the use of a shared key. In this system, both the client and the access point verify each other's identities by computing a challenge-response mechanism based on the shared key.

While many other methods exist, especially in `Enterprise` environments or with advanced protocols like `WPA3` and `Enhanced Open`, these two are the most prevalent.
### Open System Authentication

![](Authentication%20Methods-20260916-102911.png)
### Shared Key Authentication

![](Authentication%20Methods-20260916-103007.png)
___
#### Authentication with WEP

1. `Authentication request:` Initially, as it goes, the client sends the access point an authentication request.
2. `Challenge:` The access point then responds with a custom authentication response which includes challenge text for the client.
3. `Challenge Response:` The client then responds with the encrypted challenge, which is encrypted with the WEP key.
4. `Verification:` The AP then decrypts this challenge and sends back either an indication of success or failure.

![](Authentication%20Methods-20260916-103023.png)
___
#### Authentication with WPA

1. `Authentication Request:` The client sends an authentication request to the AP to initiate the authentication process.
2. `Authentication Response:` The AP responds with an authentication response, which indicates that it is ready to proceed with authentication.
3. `Pairwise Key Generation:` The client and the AP then calculate the PMK from the PSK (password).
4. `Four-Way Handshake:` The client and access point then undergo each step of the four way handshake, which involves nonce exchange, derivation, among other actions to verify that the client and AP truly know the PSK.

![](Authentication%20Methods-20260916-103154.png)

Shared key authentication type also involves [WPA3](https://documentation.meraki.com/MR/Wi-Fi_Basics_and_Best_Practices/WPA3_Encryption_and_Configuration_Guide), the latest and most secure WiFi security standard. WPA3 introduces significant improvements over its predecessors, including more robust encryption and enhanced protection against brute force attacks. One of its key features is `Simultaneous Authentication of Equals (SAE)`, which replaces the `Pre-Shared Key (PSK)` method used in WPA2, providing better protection for passwords and individual data sessions.

