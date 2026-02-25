## [[So sánh bộ 3  Wireshark & Zeek & Brim]]
# 🛠️ Brim - Network Traffic Analysis Tool

## 📌 Overview
- **Brim** là một GUI tool chuyên để **phân tích network traffic** (PCAP) và log từ **Zeek**.
- Kết hợp giao diện trực quan với backend mạnh (dựa trên Zeek + Zed lake).
- Giúp SOC/Blue Team dễ dàng điều tra incident, hunting và threat analysis.

---

## ⚡ Key Features
- **PCAP Import**: Drag & Drop file PCAP, Brim sẽ parse thành các log Zeek-readable.
- **Zeek Integration**: Tự động sinh log Zeek khi phân tích traffic.
- **Powerful Query Language (Zed)**: Lọc, tìm kiếm, pivot data dễ dàng.
- **Timeline View**: Visualization theo dòng thời gian để thấy packet & events.
- **Export**: Xuất dữ liệu thành Zeek logs, JSON, CSV.
- **Integration Friendly**: Dùng chung với Wireshark (double-click record → mở thẳng packet trong Wireshark).

---

## 📂 Use Cases
- 🔍 **Incident Response**: Import PCAP để xác định IOC (Indicators of Compromise).
- 🛡️ **Threat Hunting**: Lọc suspicious connection (ex: unusual ports, large data transfer).
- 📊 **Traffic Analysis**: Hiểu rõ behavior của malware hoặc attack pattern.
- 🤝 **Collaboration**: Export log và chia sẻ cho team khác (Blue/Red/Threat Intel).

---

## 💻 Workflow cơ bản
1. **Import PCAP**
   - File → Import PCAP hoặc drag & drop.
2. **Phân tích bằng query**
   - Dùng **Zed query** để filter.
   - Ví dụ: `count() by _path` → đếm loại Zeek log.
3. **Pivot sang Wireshark**
   - Double-click record → mở Wireshark tại gói tương ứng.
4. **Export log**
   - Xuất ra JSON/CSV hoặc Zeek logs để xài tiếp trong pipeline khác.

---

## 🔑 Zed Query Cheatsheet
| Query              | Ý nghĩa                                             |                                  |
| ------------------ | --------------------------------------------------- | -------------------------------- |
| `count()`          | Đếm tổng record                                     |                                  |
| `count() by _path` | Đếm số log theo loại Zeek (conn, dns, http, ssl, …) |                                  |
| `conn              | cut id.orig_h, id.resp_h, proto`                    | Lấy source IP, dest IP, protocol |
| `dns               | where query == "example.com"`                       | Lọc DNS query theo domain        |
| `http              | cut id.resp_h, uri, status_code`                    | Trích xuất HTTP log quan trọng   |

---

## 🔗 Resources
- [Brim Official](https://www.brimdata.io/)
- [GitHub Repo](https://github.com/brimdata/brim)
- [Docs](https://docs.brimdata.io/)

---
