## [[Cryptography]] | [[Lab Cryptography]]

___

### 🔐 **GNU Privacy Guard (GnuPG / GPG)**

- Là **một công cụ miễn phí, open-source** dựa trên chuẩn **OpenPGP (RFC 4880)**.
    
- Nó hỗ trợ cả **symmetric encryption** _và_ **asymmetric encryption**.
    
- **Use case chính:**
    
    - Mã hóa email, file.
        
    - Tạo chữ ký số (digital signature).
        
    - Quản lý key (public/private key).
        

👉 Ví dụ: Bạn gửi email bí mật → bạn dùng **public key của người nhận** để mã hóa. Người nhận dùng **private key** của họ để giải mã.

💡 Đặc biệt: GPG còn cho phép **mã hóa đối xứng** đơn giản kiểu `gpg -c file.txt`, dùng password làm key.

---

### 🔑 **OpenSSL Project**

- OpenSSL là **thư viện cryptography mạnh mẽ**, open-source, được viết bằng C.
    
- Nó cung cấp:
    
    - Thuật toán **symmetric encryption** (AES, DES, ChaCha20, …).
        
    - Thuật toán **asymmetric encryption** (RSA, ECC…).
        
    - Hash (SHA, MD5, …).
        
    - TLS/SSL protocol → cái đứng sau HTTPS.
        

👉 Bạn có thể dùng `openssl enc` để mã hóa file với AES hoặc dùng `openssl req` để tạo certificate SSL/TLS.

---

### 📊 So sánh nhanh

|Tiêu chí|**GnuPG**|**OpenSSL**|
|---|---|---|
|**Mục tiêu chính**|Bảo mật dữ liệu cá nhân (file, email, chữ ký số)|Hạ tầng mã hóa, SSL/TLS, server/client|
|**Chuẩn hỗ trợ**|OpenPGP (RFC 4880)|TLS/SSL, X.509, nhiều thuật toán|
|**Sử dụng**|`gpg` command line|`openssl` command line + library (API)|
|**Tập trung**|User-level (end-to-end)|System-level (servers, apps)|

---

### 🌍 Ứng dụng thực tế

- **GPG**: Developer hay dùng để sign commit trong GitHub/GitLab, hoặc mã hóa email với PGP.
    
- **OpenSSL**: Admin và devops dùng để tạo chứng chỉ HTTPS, hoặc mã hóa dữ liệu trong server.
    

---

## Asymmetric Encryption

---

````markdown
# 🔐 Ghi chú OpenSSL RSA

## 1. Tạo khóa riêng RSA
```bash
openssl genrsa -out private-key.pem 2048
````

- `genrsa`: Tạo khóa riêng RSA.
    
- `-out private-key.pem`: Lưu khóa riêng vào file `private-key.pem`.
    
- `2048`: Độ dài khóa = 2048 bit.
    

---

## 2. Xuất khóa công khai từ khóa riêng

```bash
openssl rsa -in private-key.pem -pubout -out public-key.pem
```

- `rsa`: Dùng thuật toán RSA.
    
- `-in private-key.pem`: Đầu vào là khóa riêng.
    
- `-pubout`: Xuất ra khóa công khai.
    
- `-out public-key.pem`: Lưu thành `public-key.pem`.
    

---

## 3. Xem chi tiết các biến RSA

```bash
openssl rsa -in private-key.pem -text -noout
```

- `-text`: Hiển thị chi tiết các thông số RSA.
    
- `-noout`: Không in lại khóa ở định dạng PEM.
    

**Các biến chính:**

- `p` → prime1 (số nguyên tố thứ nhất)
    
- `q` → prime2 (số nguyên tố thứ hai)
    
- `N` → modulus (tích `p * q`)
    
- `e` → publicExponent (số mũ công khai)
    
- `d` → privateExponent (số mũ bí mật)
    

---

## 4. Mã hóa bằng khóa công khai của người nhận

```bash
openssl pkeyutl -encrypt -in plaintext.txt -out ciphertext -inkey public-key.pem -pubin
```

- `-encrypt`: Chế độ mã hóa.
    
- `-in plaintext.txt`: File đầu vào (dữ liệu gốc).
    
- `-out ciphertext`: File đầu ra (dữ liệu đã mã hóa).
    
- `-inkey public-key.pem`: Dùng **khóa công khai** của người nhận.
    
- `-pubin`: Chỉ định rằng đây là khóa công khai.
    

---

## 5. Giải mã bằng khóa riêng của người nhận

```bash
openssl pkeyutl -decrypt -in ciphertext -inkey private-key.pem -out decrypted.txt
```

- `-decrypt`: Chế độ giải mã.
    
- `-in ciphertext`: File đã mã hóa.
    
- `-inkey private-key.pem`: Dùng **khóa riêng** của người nhận.
    
- `-out decrypted.txt`: File sau khi giải mã.
    

---
