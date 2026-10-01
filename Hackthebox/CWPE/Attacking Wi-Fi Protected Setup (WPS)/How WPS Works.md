### WPS PIN Anatomy

![](How%20WPS%20Works-20260917-152407.png)
### WPS EAP Messages

|Name|Definition|
|---|---|
|`PKe`|This is the Enrollee's (Access Point's) Diffie-Hellman public key.|
|`PKr`|This is the Registrar's (Station's/Client's) Diffie-Hellman public key.|
|`PSK1`|First four-digit portion of the PIN (10,000 possible combinations).|
|`PSK2`|Second four-digit portion of the PIN (1,000 possible combinations). The last digit is used as the checksum.|
|`KDK (Key Derivation Key)`|This is a key used in derivation for the auth key.|
|`KWK (Key Wrap Key)`|Used in the process of encrypting messages with AES.|
|`E-S1`|This is a secret 128-bit enrollee (AP) nonce value used in derivation for E-Hash1.|
|`E-S2`|This is a secret 128-bit enrollee (AP) nonce value used in derivation for E-Hash2.|
|`R-S1`|This is a secret 128-bit registrar (client/station) nonce value used in derivation for R-Hash1.|
|`R-S2`|This is a secret 128-bit registrar (client/station) nonce value used in derivation for R-Hash2.|
|`E-Hash1 (Enrollee Hash1)`|Comprised of the E-S1 nonce value, PSK1, PKe, and PKr values. Created through the HMAC-SHA-256 hashing function using the Auth Key.|
|`E-Hash2 (Enrollee Hash2)`|Comprised of the E-S2 nonce value, PSK2, PKe, and PKr values. Created through the HMAC-SHA-256 hashing function using the Auth Key.|
|`R-Hash1 (Registrar Hash1)`|Comprised of the R-S1 nonce value, PSK1, PKe, and PKr values. Created through the HMAC-SHA-256 hashing function using the Auth Key.|
|`R-Hash2 (Registrar Hash2)`|Comprised of the R-S2 nonce value, PSK2, PKe, and PKr values. Created through the HMAC-SHA-256 hashing function using the Auth Key.|
|`Auth Key`|Derived from the KDK, PSK1, and PSK2 values.|
|`WPA-PSK`|This is the final disclosed pre-shared key (aka password) used to authenticate the client.|
![](How%20WPS%20Works-20260917-152420.png)

|Message|Description|
|---|---|
|`EAPOL-Start`|The connected client initiates the series of EAP messages.|
|`EAP Request Identity`|The access point requests the connected client's identity.|
|`EAP Response Identity`|The client sends the access point its identity as requested.|
|`EAP M1 Message`|The access point sends the client their Diffie-Hellman public key (PKe).|
|`EAP M2 Message`|The client then sends the Access point the their Diffie-Hellman public key (PKr).|
|`EAP M3 Message`|The access point sends the client the E-Hash1 and E-Hash2 values.|
|`EAP M4 Message`|The client then sends the access point the R-Hash1, R-Hash2, and R-S1 nonce value encrypted with AES.|
|`EAP M5 Message`|The access point sends the client the E-S1 nonce value encrypted with AES.|
|`EAP M6 Message`|The client sends the access point the R-S2 nonce value encrypted with AES.|
|`EAP M7 Message`|If the PIN is correct, the access point sends the client the E-S2 value and the WPA-PSK encrypted with AES.|
|`EAP M8 Message`|The client then sends the WPA-PSK back to the access point to begin the WPA handshake process.|
