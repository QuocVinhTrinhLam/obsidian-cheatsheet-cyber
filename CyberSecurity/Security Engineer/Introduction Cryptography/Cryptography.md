## [[Lab Cryptography]] | [[GNU Privacy Guard & OpenSSL Project]]

___
## Symmetric Encryption

Symmetric encryption (mã hóa đối xứng) là một phương pháp mã hóa dữ liệu **dùng chung một khóa bí mật** cho cả hai quá trình:

- **Encryption (mã hóa)**: Biến dữ liệu gốc (plaintext) → dữ liệu mã hóa (ciphertext).
    
- **Decryption (giải mã)**: Biến dữ liệu mã hóa (ciphertext) → dữ liệu gốc (plaintext).
    

**Điểm quan trọng:** Cùng một **key** được dùng ở cả hai đầu (sender và receiver).

---

### 🔑 **Đặc điểm chính**

- **Nhanh**: Thuật toán nhẹ hơn so với mã hóa bất đối xứng (asymmetric).
    
- **Khó khăn lớn nhất**: **Quản lý và phân phối khóa** (key distribution). Làm sao gửi key cho người nhận mà không bị lộ?
    
- **Ứng dụng thực tế**: SSL/TLS (trong handshake sau khi trao đổi key), mã hóa file, database, VPN...
    

---

### 📚 **Các thuật toán phổ biến**

- **AES (Advanced Encryption Standard)** → Chuẩn phổ biến nhất hiện nay.
    
- **DES (Data Encryption Standard)** → Cũ, không còn an toàn.
    
- **3DES** → Cũng cũ, chậm.
    
- **Blowfish, Twofish** → Các lựa chọn khác.
    

---

### 🔍 **Ví dụ minh họa**

Giả sử:

- Plaintext: `HELLO`
    
- Key: `12345`
    
- Sau mã hóa bằng AES → Ciphertext: `4F7A91C...`
    
- Giải mã với cùng key → `HELLO`
    

---

### ⚖ **Ưu & Nhược điểm**

✔ Ưu:

- Tốc độ nhanh, dùng cho dữ liệu lớn.  
    ✖ Nhược:
    
- Vấn đề phân phối khóa (phải có kênh an toàn để trao đổi key).
    
- Nếu key bị lộ → mất an toàn toàn bộ dữ liệu.
    

---

🔥 **Tip để nhớ:** Symmetric = **Same Key** (cả hai đầu dùng chung một khóa).

---
## Asymmetric Encryption

### 🔑 **Asymmetric Encryption (Mã hóa bất đối xứng)**

Khác với symmetric (cùng 1 key), **asymmetric dùng 2 key khác nhau** nhưng liên quan toán học chặt chẽ:

- **Public key (khóa công khai):** Ai cũng có thể biết.
    
- **Private key (khóa bí mật):** Chỉ chủ sở hữu giữ.
    

---

### 🔄 **Cách hoạt động cơ bản**

1. Nếu **A muốn gửi dữ liệu bí mật cho B**:
    
    - A mã hóa bằng **public key của B**.
        
    - Chỉ B (với private key của mình) mới giải mã được.
        
2. Nếu **B muốn chứng minh dữ liệu do mình gửi** (chữ ký số):
    
    - B ký dữ liệu bằng **private key**.
        
    - Ai cũng có thể kiểm tra bằng **public key** của B.
        

![[Screenshot 2025-08-27 at 17.34.55.png]]

---

### 📚 **Thuật toán phổ biến**

- **RSA** (kinh điển, dễ hiểu).
    
- **ECC (Elliptic Curve Cryptography)**: mới hơn, nhanh và an toàn hơn RSA với key nhỏ.
    
- **DSA**: dùng chủ yếu cho chữ ký số.
    

---

### ⚖ So sánh với Symmetric

|Tiêu chí|Symmetric|Asymmetric|
|---|---|---|
|**Key**|1 key (same)|2 key (public/private)|
|**Tốc độ**|Nhanh (AES, DES)|Chậm (RSA, ECC)|
|**Khó khăn**|Chia sẻ key an toàn|Tính toán nặng hơn|
|**Ứng dụng**|Mã hóa dữ liệu lớn (file, database)|Key exchange, chữ ký số, certificate|

---

### 🌍 Ứng dụng thực tế

- **HTTPS / SSL/TLS**: Ban đầu dùng asymmetric để trao đổi symmetric key, sau đó dùng symmetric để mã hóa traffic.
    
- **GPG**: Dùng asymmetric để chia sẻ key và tạo chữ ký số.
    
- **SSH**: Xác thực với public/private key.
    
- **Blockchain (Bitcoin, Ethereum)**: Dùng asymmetric (ECC) để tạo ví và ký giao dịch.
    

---

🔥 **Tip nhớ nhanh**:

- **Symmetric = Speed** (nhanh, 1 key).
    
- **Asymmetric = Security/Sharing** (2 key, dễ chia sẻ public key).
    

---
## Diffie-Hellman Key Exchange

### 🌱 **Diffie–Hellman Key Exchange là gì?**

- Là một **thuật toán trao đổi khóa** (key exchange algorithm).
    
- Hai bên (Alice và Bob) có thể tạo ra **một secret key chung** để dùng trong mã hóa **đối xứng**, ngay cả khi giao tiếp qua kênh không an toàn.
    
- Điểm hay: secret key **không bao giờ truyền trực tiếp trên mạng** → kẻ nghe lén cũng khó lấy được.
    

---

### 🔑 **Ý tưởng chính**

Dựa vào tính chất toán học của **modular exponentiation** và **Discrete Logarithm Problem** (rất khó giải ngược).

---

### 🔄 **Quy trình đơn giản**

1. **Chọn số công khai chung**:
    
    - Một số nguyên tố lớn `p`.
        
    - Một số cơ sở (generator) `g`.
        
    
    👉 Cả `p` và `g` đều **công khai**, ai cũng biết.
    
2. **Alice và Bob chọn secret riêng**:
    
    - Alice chọn số bí mật `a`.
        
    - Bob chọn số bí mật `b`.
        
3. **Tạo khóa công khai gửi cho nhau**:
    
    - Alice tính: `A = g^a mod p`.
        
    - Bob tính: `B = g^b mod p`.
        
    
    (Rồi Alice gửi `A` cho Bob, Bob gửi `B` cho Alice).
    
4. **Tạo khóa bí mật chung**:
    
    - Alice tính: `s = B^a mod p`.
        
    - Bob tính: `s = A^b mod p`.
        
    
    👉 Do tính chất toán học: `s` của Alice và Bob sẽ giống nhau (`g^(ab) mod p`).
    

Kẻ tấn công dù thấy `A`, `B`, `p`, `g` cũng **không tính lại được `s`** (trừ khi giải được discrete log → cực khó).

---

### 📊 Ví dụ mini (với số nhỏ cho dễ hiểu)

- Chọn `p = 23`, `g = 5`.
    
- Alice chọn `a = 6`. Bob chọn `b = 15`.
    
- Alice tính `A = 5^6 mod 23 = 8`.
    
- Bob tính `B = 5^15 mod 23 = 19`.
    
- Alice tính secret: `s = 19^6 mod 23 = 2`.
    
- Bob tính secret: `s = 8^15 mod 23 = 2`.
    

👉 Khóa chung = `2`.

---

### ⚖ Đặc điểm

✔ Ưu điểm:

- Khóa bí mật không bị truyền trực tiếp.
    
- Cơ sở cho nhiều giao thức (TLS, VPN, SSH…).
    

✖ Nhược điểm:

- Bị **Man-in-the-Middle (MITM attack)** nếu không có xác thực (ví dụ attacker chen vào đổi `A` và `B`).
    
- Giải pháp: dùng cùng với **digital signatures** hoặc **public key infrastructure (PKI)** để đảm bảo xác thực.
    

---
## Hàm băm mật mã

#### Tổng quan
- **Hàm băm mật mã**: Thuật toán nhận dữ liệu kích thước bất kỳ, trả về giá trị cố định gọi là **message digest** hoặc **checksum**.
- **Ví dụ**: SHA256 tạo checksum 256 bit (32 byte), biểu diễn bằng 64 chữ số thập lục phân (mỗi chữ số = 4 bit).

#### Ví dụ thực tế
- Tính SHA256 cho tệp:
  - `abc.txt` (4 byte): `c38bb113c89d8fec6475a9936411007c45563ecb7ce8acd5db7fb58c0872bda0`
  - `debian-hurd.img.tar.xz` (275 MB): `饼317ff0150e0d64b70284b28c97bb788310585ea7ac46cc8139d5a3c850dea55`
  - `Win11_English_x64v1.iso` (5.2 GB): `4bc6c7e7c61af4b5d1b086c5d279947357cff45c2f82021bb58628c2503eb64e`
- **Đặc điểm**: Kích thước checksum luôn cố định (256 bit) bất kể kích thước tệp.

#### Ứng dụng
1. **Lưu trữ mật khẩu**:
   - Lưu hash của mật khẩu thay vì văn bản gốc.
   - Nếu xảy ra vi phạm dữ liệu, kẻ tấn công chỉ lấy được hash, không phải mật khẩu gốc.
2. **Phát hiện sửa đổi**:
   - Thay đổi nhỏ trong tệp dẫn đến checksum hoàn toàn khác.
   - Ví dụ: `text1.txt` (TryHackMe) và `text2.txt` (tryHackMe) khác 1 bit (T/t), nhưng SHA256 khác biệt:
     - `text1.txt`: `f4616fd825a10ded9af58fbaee09f3e31751d15591f9323ea68b03a0e8ac3783`
     - `text2.txt`: `9ffa3533ee33998aeb1df76026f8031c8af6ccabd8393eca002d5b7471a0b536`
   - Giúp phát hiện giả mạo hoặc lỗi truyền tệp.

#### Hàm băm an toàn
- **An toàn**: SHA224, SHA256, SHA384, SHA512, RIPEMD160.
- **Không an toàn**: MD5, SHA-1 (có thể tạo va chạm hash, tức là tạo tệp khác với cùng checksum).

#### HMAC (Hash-based Message Authentication Code)
- **HMAC**: Mã xác thực thông điệp sử dụng khóa bí mật kết hợp với hàm băm.
- **Yêu cầu** (theo RFC2104):
  - Khóa bí mật.
  - Inner pad (ipad): Chuỗi hằng số (0x36 lặp lại B lần, B phụ thuộc hàm băm).
  - Outer pad (opad): Chuỗi hằng số (0x5C lặp lại B lần).
- **Các bước tính HMAC**:
  1. Thêm số 0 vào khóa để đạt độ dài B (bằng độ dài ipad).
  2. Tính \( \text{key} \oplus \text{ipad} \) (XOR).
  3. Gắn thông điệp vào kết quả bước 2.
  4. Áp dụng hàm băm lên luồng byte bước 3.
  5. Tính \( \text{key} \oplus \text{opad} \) (XOR).
  6. Gắn kết quả bước 4 vào kết quả bước 5.
  7. Áp dụng hàm băm để tạo HMAC.
- **Công thức**: \( H(K \oplus \text{opad}, H(K \oplus \text{ipad}, \text{text})) \).

#### Thực hành HMAC trên Linux
- **Tính HMAC**:

  ```bash
  hmac256 s!Kr37 message.txt
  ```
 
- Kết quả: `3ec65b7e80c5bf2e623e52e0528f1c6a74f605b10616621ba1c22a89fb244e65`
  
  ```bash
  hmac256 1234 message.txt
  ```
  
- Kết quả: `4b6a2783631180fca6128592e3d17fb5bff6b0e563ad8f1c6afc1050869e440f`

  ```bash
  sha256hmac message.txt --key s!Kr37
  ```
  
- Kết quả: `3ec65b7e80c5bf2e623e52e0528f1c6a74f605b10616621ba1c22a89fb244e65`

  ```bash
  sha256hmac message.txt --key 1234
  ```
  
- Kết quả: `4b6a2783631180fca6128592e3d17fb5bff6b0e563ad8f1c6afc1050869e440f`

## **HMAC (Hash-based Message Authentication Code)**

- Là **cơ chế xác thực thông điệp** (Message Authentication Code – MAC).
    
- Dùng **hàm băm (hash function)** kết hợp với một **secret key** để tạo ra “mã xác thực” cho dữ liệu.
    

👉 Nghĩa là:

- Hash bình thường chỉ chứng minh được _tính toàn vẹn_ (data không đổi).
    
- HMAC chứng minh được cả _tính toàn vẹn_ **và** _tính xác thực_ (chỉ ai có key mới tạo ra được HMAC đúng).
    

---

### 🔄 **Cách hoạt động (simplified)**

1. Người gửi và người nhận **chia sẻ trước một secret key K**.
    
2. Người gửi tính:
    
    ```
    HMAC = Hash( (K ⊕ opad) ∥ Hash( (K ⊕ ipad) ∥ message ) )
    ```
    
    (opad = outer padding, ipad = inner padding – kỹ thuật để trộn key vào hash).
    
3. Gửi `message` kèm `HMAC`.
    
4. Người nhận dùng key K tính lại → nếu HMAC khớp thì:
    
    - Message chưa bị thay đổi.
        
    - Message chắc chắn đến từ người có key.
        

---

### 📚 **Thuật toán HMAC thường dùng**

- **HMAC-MD5** (cũ, không nên dùng).
    
- **HMAC-SHA1** (an toàn trung bình).
    
- **HMAC-SHA256 / HMAC-SHA512** (chuẩn phổ biến hiện nay).
    

---

### 🌍 **Ứng dụng thực tế**

- **TLS/SSL**: bảo vệ traffic mạng.
    
- **API Authentication**: Nhiều API (AWS, Azure, Google Cloud) dùng HMAC để sign request.
    
- **VPN, IPSec**: Xác thực gói tin.
    

---

### 🆚 **Hash vs HMAC**

|Đặc điểm|Hash|HMAC|
|---|---|---|
|**Key**|Không dùng key|Dùng secret key|
|**Đảm bảo**|Chỉ toàn vẹn|Toàn vẹn + xác thực|
|**Ứng dụng**|Checksum file|Bảo mật truyền thông, API auth|

---

## PKI and SSL/TLS

### 🏛 **PKI (Public Key Infrastructure)**

PKI giống như một **hệ sinh thái quản lý khóa công khai**. Nó giải quyết vấn đề:  
👉 “Làm sao để tin rằng public key này thật sự thuộc về đúng người/website?”

### Thành phần chính của PKI:

- **CA (Certificate Authority):** Tổ chức cấp phát chứng chỉ số (VD: DigiCert, GlobalSign).
    
- **RA (Registration Authority):** Xác minh danh tính trước khi CA cấp chứng chỉ.
    
- **Certificate (chứng chỉ số):** Gói chứa public key + thông tin chủ sở hữu + chữ ký CA.
    
- **CRL/OCSP:** Danh sách chứng chỉ bị thu hồi (revoked) hoặc kiểm tra trạng thái.
    

📌 **Vai trò PKI**: Quản lý, phân phối và tin cậy khóa công khai.

---

### 🌐 **SSL/TLS (Secure Sockets Layer / Transport Layer Security)**

SSL (cũ) → TLS (phiên bản mới, chuẩn hiện tại). Đây là **giao thức bảo mật truyền thông trên Internet** (HTTPS chính là HTTP chạy trên TLS).

### Các bước chính trong TLS handshake (phiên bản đơn giản hóa):

1. **Client Hello:** Trình duyệt gửi “xin chào”, gồm danh sách thuật toán mã hóa nó hỗ trợ.
    
2. **Server Hello + Certificate:** Server chọn thuật toán, gửi về chứng chỉ số (PKI).
    
3. **Key Exchange:** Dùng RSA hoặc Diffie-Hellman/ECDH để thỏa thuận ra một **session key**.
    
4. **Session Key (Symmetric):** Sau khi có key chung, dữ liệu trao đổi dùng **AES (hoặc symmetric cipher)** để mã hóa (nhanh hơn asymmetric).
    
5. **Secure Communication:** Tất cả request/response được mã hóa + kiểm tra tính toàn vẹn (MAC/HMAC).
    

📌 **Vai trò TLS:** Đảm bảo **CIA** (Confidentiality, Integrity, Authentication).

---

### 🔗 PKI + TLS kết nối như nào?

- PKI cung cấp **chứng chỉ số (certificate)** để client tin rằng server là thật.
    
- TLS dùng chứng chỉ đó để thiết lập **kênh liên lạc bảo mật**.
    

👉 Nói ngắn gọn:

- **PKI = quản lý lòng tin (who owns which key)**.
    
- **TLS = tạo đường hầm an toàn để truyền dữ liệu**.
    

---
