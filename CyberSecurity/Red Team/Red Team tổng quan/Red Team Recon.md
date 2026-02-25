
nslookup, dig, host, whois, traceroute

---

## 🧾 1. `nslookup` (Name Server Lookup)

### ✅ Mục đích:

Truy vấn DNS – tìm địa chỉ IP từ tên miền (hoặc ngược lại).

### ✅ Cách dùng:

```bash
nslookup example.com
```

### ✅ Ví dụ:

```bash
nslookup google.com
```

Kết quả sẽ trả về IP của `google.com`.

### ✅ Một số lệnh nâng cao:

```bash
nslookup
> set type=MX
> example.com
```

(Lấy bản ghi Mail Exchange – dùng trong truy vết email server)

---

## 🔍 2. `dig` (Domain Information Groper)

### ✅ Mục đích:

Truy vấn DNS chuyên sâu hơn `nslookup`. Trả kết quả chi tiết, rõ ràng – thường dùng trong bảo mật, pentest.

### ✅ Cách dùng:

```bash
dig example.com
```

### ✅ Một số ví dụ nâng cao:

```bash
dig example.com MX      # Lấy bản ghi Mail Exchange
dig @8.8.8.8 example.com A   # Truy vấn DNS bằng server 8.8.8.8
dig +short example.com       # Chỉ hiện IP
```

### ✅ Ưu điểm:

- Chi tiết hơn `nslookup`
    
- Được ưa dùng bởi các chuyên gia
    

---

## 🌐 3. `host`

### ✅ Mục đích:

Truy vấn DNS đơn giản – nhanh gọn, dễ dùng hơn `dig`.

### ✅ Cách dùng:

```bash
host example.com
```

### ✅ Kết quả:

Trả về IP hoặc thông tin tên miền.

### ✅ Một số ví dụ nâng cao:

```bash
host -t mx example.com
host -a example.com
```

---

## 👤 4. `whois`

### ✅ Mục đích:

Lấy thông tin đăng ký tên miền hoặc địa chỉ IP (tên chủ sở hữu, email, ngày hết hạn, v.v.)

### ✅ Cách dùng:

```bash
whois example.com
```

### ✅ Ví dụ:

```bash
whois facebook.com
```

### ✅ Kết quả:

- Tên công ty sở hữu
    
- Ngày đăng ký/hết hạn
    
- Tên người đăng ký, email (nếu không bị ẩn)
    

### ⚠️ Lưu ý:

- Nếu bạn query nhiều lần, bạn có thể bị **rate limit** hoặc bị yêu cầu dùng proxy.
    

---

## 🛣️ 5. `traceroute` (trên Linux) / `tracert` (trên Windows)

### ✅ Mục đích:

Xem đường đi của gói tin từ máy bạn → server đích, qua các router trung gian nào.

### ✅ Cách dùng:

```bash
traceroute example.com
```

### ✅ Ví dụ:

```bash
traceroute google.com
```

### ✅ Kết quả:

Hiện ra danh sách IP và thời gian phản hồi của từng "hop" (router trung gian). Rất hữu ích khi debug kết nối mạng.

---

## 🔁 Tổng so sánh nhanh:

|Công cụ|Mục đích chính|Dễ dùng|Trả kết quả chi tiết|
|---|---|---|---|
|`nslookup`|Truy vấn DNS (cơ bản)|✅|❌|
|`dig`|Truy vấn DNS (chuyên sâu)|Trung bình|✅|
|`host`|Truy vấn DNS (đơn giản, gọn)|✅|❌|
|`whois`|Tra thông tin chủ sở hữu domain/IP|✅|✅|
|`traceroute`|Xem đường đi gói tin qua các router|Trung bình|✅|

---

