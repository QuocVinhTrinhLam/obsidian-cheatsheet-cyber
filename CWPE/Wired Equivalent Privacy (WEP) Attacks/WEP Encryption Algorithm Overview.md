![](WEP%20Encryption%20Algorithm%20Overview-20260921-140554.png)

The algorithm for WEP follows a fairly standard procedure for generating a keystream through the RC4 algorithm, which then undergoes a bitwise operation with the packet plaintext and cyclic redundancy check. It can be broken down into the following steps:

- The 24-bit `Initialization Vector (IV)` is generated.
- The `40-bit` or `104-bit Key` is combined with the initialization vector to make the `Seed`.
- The `Seed` is passed through the stages of the RC4 algorithm, which includes the Key Scheduling Algorithm and the Pseudo Random Generation Algorithm, to create the `Keystream`.
- The `Cyclic Redundancy Check` is calculated and appended to the `Packet Plain Text`, forming the `ICV message`.
- The unencrypted `ICV message` and `Keystream` undergo a `XOR Bitwise Operation` to produce the `Final Ciphertext`.
- The IV is concatenated with the final ciphertext, resulting in the `final message` to be transmitted.
