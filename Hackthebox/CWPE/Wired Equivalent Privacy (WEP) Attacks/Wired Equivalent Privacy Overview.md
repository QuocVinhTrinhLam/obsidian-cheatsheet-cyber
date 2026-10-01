## RC4 Algorithm

In cryptography, `RC4 (Rivest Cipher 4)`, also known as `ARC4` or `ARCFOUR (Alleged RC4)`, is a stream cipher. It was designed by Ron Rivest of [RSA Security](https://en.wikipedia.org/wiki/RSA_Security) in 1987 and became part of several commonly used encryption protocols and standards (including WEP) due to its simplicity and high speed.

RC4 is a symmetric cipher, which means the same key is used for both encryption and decryption. It generates a stream of bits that are XORed with the plaintext to produce the ciphertext. To decrypt the data, the ciphertext is XORed with the same key stream to recover the plaintext.

RC4 consists of two key components:

1. Key Scheduling Algorithm (KSA)
2. Pseudo Random Generation Algorithm (PRGA)

The `Key Scheduling Algorithm` initializes the state table using the WEP key and the initialization vector (IV). The `Pseudo Random Generation Algorithm` produces the keystream used for the encryption and decryption process. In the upcoming section, we will delve deeper into the RC4 algorithm, exploring its mechanisms and functionality in greater detail.
## WEP Authentication

WEP supports two types of authentication systems: `Open` and `Shared`. In open authentication, a client does not provide any credentials when connecting to the access point (AP). However, to encrypt and decrypt data frames, the client must have the correct key.

![](Wired%20Equivalent%20Privacy%20Overview-20260921-140416.png)

Below is a step-by-step description of the shared WEP authentication process, which can be visualized in the diagram above:

1. `Authentication Request`: The process begins with the client sending an authentication request to the access point.
2. `Challenge`: The access point responds with a custom authentication response that includes challenge text for the client.
3. `Challenge Response`: The client then replies with the encrypted challenge, which is encrypted using the WEP key.
4. `Verification`: The AP decrypts the challenge, and sends back an indication of success or failure.