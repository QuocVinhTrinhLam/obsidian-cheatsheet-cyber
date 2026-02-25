## [[Wireshark]] [[Tshark]]

---

# 📌 Tshark Cheat Sheet

## ✅ **Tổng quan**

- **Tshark** = Wireshark CLI
    
- Dùng để **capture**, **phân tích gói tin** và **xuất dữ liệu** (PCAP, JSON, CSV).
    
- Hữu ích cho **automation**, **remote capture**, **SOC**, **pentest**.
    

---

## 🔍 **Hiển thị Interface**

```bash
tshark -D
```

Liệt kê tất cả interface có thể capture.

---

## 🎯 **Capture Gói Tin**

- Capture từ interface `eth0`:
    

```bash
tshark -i eth0
```

- Capture **100 gói tin**:
    

```bash
tshark -i eth0 -c 100
```

- Capture trong **10 giây**:
    

```bash
tshark -i eth0 -a duration:10
```

- Capture và **lưu vào file pcap**:
    

```bash
tshark -i eth0 -w output.pcap
```

---

## 📂 **Đọc File PCAP**

```bash
tshark -r file.pcap
```

---

## 🔍 **Filter**

### Capture Filter (`-f`)

Giống tcpdump (BPF syntax):

```bash
tshark -i eth0 -f "tcp port 80"
```

### Display Filter (`-Y`)

Giống Wireshark:

```bash
tshark -i eth0 -Y "http.request"
```

---

## 📤 **Xuất Dữ Liệu**

- **JSON**:
    

```bash
tshark -r file.pcap -T json
```

- **CSV** (chỉ field HTTP):
    

```bash
tshark -r file.pcap -T fields -e http.host -e http.user_agent -E header=y -E separator=,
```

---

## ⚙ **Option Quan Trọng**

|Option|Mô tả|
|---|---|
|`-i <iface>`|Chọn interface để capture|
|`-D`|Liệt kê interface|
|`-c <num>`|Capture số gói tin rồi dừng|
|`-a duration:10`|Capture trong 10 giây|
|`-f`|Capture filter (tcpdump syntax)|
|`-Y`|Display filter (Wireshark syntax)|
|`-T json`|Xuất dữ liệu JSON|
|`-T fields`|Xuất dữ liệu theo field|
|`-w file.pcap`|Lưu dữ liệu raw dạng pcap|

---

## ✅ **Use Case Thường Gặp**

|Tình huống|Lệnh|
|---|---|
|Capture HTTP traffic|`tshark -i eth0 -Y "http"`|
|Lọc chỉ GET request|`tshark -i eth0 -Y "http.request.method == GET"`|
|Tìm packet chứa từ khóa "login"|`tshark -i eth0 -Y "frame contains login"`|
|Xuất tất cả IP nguồn|`tshark -r file.pcap -T fields -e ip.src`|

---

## 🔗 **Tham khảo**

- [Wireshark Display Filter Reference](https://www.wireshark.org/docs/dfref/)
    
- [Tshark Man Page](https://www.wireshark.org/docs/man-pages/tshark.html)
    

---
