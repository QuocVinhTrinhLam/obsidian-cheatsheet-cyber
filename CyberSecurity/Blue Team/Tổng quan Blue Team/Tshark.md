## [[Wireshark]] [[Wireshark 🦈]]

Tshark là **phiên bản dòng lệnh (CLI) của Wireshark**, được dùng để **capture và phân tích gói tin** trong mạng, phù hợp cho môi trường không có GUI hoặc khi bạn cần tự động hóa qua script.

---

## ✅ **Tổng quan về Tshark**

- **Thuộc bộ Wireshark** → khi cài Wireshark sẽ có luôn Tshark.
    
- **Hoạt động trên terminal** → dùng để sniff, phân tích traffic hoặc xuất dữ liệu dưới dạng text, CSV, JSON.
    
- **Hữu ích cho automation & remote capture** → nhẹ hơn Wireshark vì không có giao diện đồ họa.
    

---

## 🔑 **Tính năng chính**

- **Capture traffic** từ interface mạng.
    
- **Filter gói tin** bằng **capture filter** (BPF) và **display filter** giống Wireshark.
    
- **Export dữ liệu** ra nhiều format (PCAP, JSON, CSV, PDML).
    
- **Thích hợp cho script** để phân tích nhanh (automation trong SOC hoặc pentest).
    

---

## ⚡ **Cách sử dụng cơ bản**

### 1. **Hiển thị interface có thể capture**

```bash
tshark -D
```

### 2. **Capture traffic từ interface eth0**

```bash
tshark -i eth0
```

### 3. **Capture với giới hạn số gói**

```bash
tshark -i eth0 -c 100
```

### 4. **Dùng Display Filter (giống Wireshark)**

```bash
tshark -i eth0 -Y "http.request"
```

### 5. **Capture và lưu thành file .pcap**

```bash
tshark -i eth0 -w output.pcap
```

### 6. **Đọc file pcap**

```bash
tshark -r file.pcap
```

---

## 📊 **Xuất dữ liệu phân tích**

- Xuất ra **JSON**:
    

```bash
tshark -r file.pcap -T json
```

- Xuất ra **CSV (HTTP fields)**:
    

```bash
tshark -r file.pcap -T fields -e http.host -e http.user_agent -E header=y -E separator=,
```

---

## 🔍 **Một số option hay**

|Option|Ý nghĩa|
|---|---|
|`-i <iface>`|Chọn interface để capture|
|`-D`|Liệt kê tất cả interface|
|`-c <num>`|Capture gói tin rồi dừng|
|`-a duration:10`|Capture trong 10 giây rồi dừng|
|`-f`|Capture filter (giống tcpdump, ex: `tcp port 80`)|
|`-Y`|Display filter (giống Wireshark)|
|`-T json`|Output JSON|
|`-w file.pcap`|Lưu dữ liệu raw dạng pcap|

---

## ✅ **Khi nào dùng Tshark thay vì Wireshark?**

- Khi **không có GUI** (remote server, container).
    
- Khi cần **script automation** hoặc chạy trong CI/CD.
    
- Khi capture **số lượng lớn** packet mà không muốn tốn tài nguyên.
    

---
