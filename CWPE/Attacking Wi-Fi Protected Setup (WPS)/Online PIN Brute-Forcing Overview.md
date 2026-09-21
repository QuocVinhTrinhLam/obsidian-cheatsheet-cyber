![](Online%20PIN%20Brute-Forcing%20Overview-20260921-081304.png)

In online brute-forcing attacks, we already know some values and generate others. These are as follows:

- We know the PKe and generate the PKr ourselves. This allows us to generate the proper response for the R-Hash1 and R-Hash2 values.
- We generate the R-S1 and R-S2 nonce values ourselves.
- We receive the E-Hash1 and E-Hash2 values from the access point during the M3 message.

However, during these attacks we do not know:

- The true PIN (PSK1 and PSK2 values). We guess this through the 11,000 combinations.
- The E-S1 and E-S2 nonce values. We receive these from the access point in the M5 and M7 message. Of course, only if we guess the PIN correctly.
- The WPA-PSK. This is the resulting success during the M7 message upon guessing the correct PIN.
