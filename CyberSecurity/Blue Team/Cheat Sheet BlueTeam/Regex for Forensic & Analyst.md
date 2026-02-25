
---

# 🔍 Regex Cheat Sheet cho Packet Analysis & Forensic

## ✅ **Tổng quan**

- **Regex** cực hữu ích trong việc lọc dữ liệu từ log, file PCAP, hoặc xuất từ Tshark.
    
- Sử dụng với **grep -E**, **awk**, **sed**, hoặc trong **Wireshark display filter** (có giới hạn).
    

---

## 📌 **1. Email Address**

### Basic (đơn giản)

```
\w+@\w+\.\w+
```

### Chuẩn RFC cơ bản (chính xác hơn)

```
[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
```

---

## 📌 **2. IPv4 Address**

```
([0-9]{1,3}\.){3}[0-9]{1,3}
```

> Bắt các địa chỉ IPv4 như `192.168.1.10`.

---

## 📌 **3. IPv6 Address**

```
([0-9a-fA-F]{0,4}:){2,7}[0-9a-fA-F]{0,4}
```

---

## 📌 **4. URL**

```
https?:\/\/[^\s]+
```

> Match URL bắt đầu bằng `http://` hoặc `https://`.

---

## 📌 **5. Domain Name**

```
[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
```

---

## 📌 **6. MAC Address**

```
([0-9A-Fa-f]{2}:){5}[0-9A-Fa-f]{2}
```

---

## 📌 **7. Credit Card Numbers**

### Visa / MasterCard (đơn giản)

```
(?:4[0-9]{12}(?:[0-9]{3})?)|(?:5[1-5][0-9]{14})
```

---

## 📌 **8. Hash**

- **MD5**:
    

```
[a-fA-F0-9]{32}
```

- **SHA1**:
    

```
[a-fA-F0-9]{40}
```

- **SHA256**:
    

```
[a-fA-F0-9]{64}
```

---

## 📌 **9. Username & Password Patterns**

- Username (alphanumeric + underscore):
    

```
\w{3,16}
```

- Password (8–20 ký tự, có chữ và số):
    

```
(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,20}
```

---

## 📌 **10. Extract Form Data**

```
(Form item|Value):\s*".*"
```

> Thường thấy khi dump từ HTTP POST hoặc form submissions.

---

## ✅ **Cách sử dụng với grep**

- Tìm tất cả email trong file:
    

```bash
grep -E "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" fullcapture.txt
```

- Tìm tất cả IP trong PCAP text:
    

```bash
grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}" fullcapture.txt
```

---

## 🔗 **Công cụ hỗ trợ kiểm thử regex**

- [regex101.com](https://regex101.com/)
    
- [CyberChef](https://gchq.github.io/CyberChef/)
    

---
