## [[Cryptography]] | [[GNU Privacy Guard & OpenSSL Project]]

___
## Task 02: Symmetric Encryption
### Quote 1
`gpg --output original_msg.txt --decrypt quote01.txt.gpg`
![[Screenshot 2025-08-27 at 17.19.46.png]]
Result: `waste` 

### Quote 2
`openssl aes-256-cbc -d -in quote2 -out original_message.txt`
![[Screenshot 2025-08-27 at 17.24.00.png]]

### Quote 3
`gpg --output original_message2.txt --decrypt quote03.txt.gpg`
![[Screenshot 2025-08-27 at 17.26.45.png]]
___
## Task 03: Asymmetric Encryption

### Quest 1:
`openssl pkeyutl -decrypt -in ciphertext_message -inkey private-key-bob.pem -out decrypted.txt`
![[Screenshot 2025-08-27 at 17.42.44.png]]

### Quest 2,3:
`openssl rsa -in private-key-bob.pem -text -noout`
Prime 1: last byte of p = e7
Prime2: last byte of q = 27
![[Screenshot 2025-08-27 at 17.50.24.png]]

___

## Task 04: Diffie-Hellman key exchange algorithm

### Question 1: 
`openssl dhparam -in dhparams.pem -text -noout`
![[Screenshot 2025-08-27 at 18.14.58.png]]
### Question 2:
![[Screenshot 2025-08-27 at 18.15.17.png]]

___
## Task 05: Hashing

### Question 1:
`sha256sum order.json`
![[Screenshot 2025-08-27 at 18.23.52.png]]

### Question 2:
Đổi `amount` của file order.json
![[Screenshot 2025-08-27 at 18.24.49.png]]![[Screenshot 2025-08-27 at 18.25.20.png]]

### Question 3:
`hmac256 -key order.txt`
![[Screenshot 2025-08-27 at 18.27.15.png]]

___

## Task 06:

### Question 1,2:
 `openssl req -x509 -newkey -nodes rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 365` to create 
 `openssl x509 -in cert.pem -text` to view
![[Screenshot 2025-08-27 at 18.36.13.png]]