## [[So sánh bộ 3  Wireshark & Zeek & Brim]]
# 🦈 Wireshark - Tổng Quan

## 1. Wireshark là gì?
- Công cụ **network protocol analyzer** mã nguồn mở, cross-platform.  
- Dùng để **bắt, phân tích và debug** gói tin mạng theo thời gian thực hoặc từ file capture.  
- Được sử dụng trong:
  - 🛡️ An ninh mạng (Blue Team, Incident Response).
  - 🧑‍💻 Pentest & Red Team.
  - ⚙️ Debug ứng dụng & hệ thống.
  - 📡 Quản trị mạng.

---

## 2. Chức năng chính
- **Capture gói tin**: từ card mạng (Ethernet, Wi-Fi, VPN…).
- **Phân tích protocol**: hỗ trợ hàng ngàn giao thức (TCP, UDP, HTTP, DNS, SSL/TLS, SMB...).
- **Filter mạnh mẽ**: 
  - Display Filter (hiển thị theo điều kiện).
  - Capture Filter (giới hạn ngay khi capture).
- **Reassembly**: ghép lại stream TCP, HTTP objects, VoIP calls.
- **Export**: trích xuất file, đối tượng (vd: file ảnh từ HTTP stream).

---

## 3. Ưu điểm
- Mã nguồn mở, miễn phí.
- GUI trực quan, có cả CLI (`tshark`).
- Bộ filter cực chi tiết, hỗ trợ regex-like.
- Hỗ trợ multi-platform: Windows, Linux, macOS.

---

## 4. Nhược điểm
- Bắt gói tin yêu cầu **quyền root/admin**.
- Có thể gây **lag hệ thống** nếu capture quá lâu / nhiều traffic.
- Không tự động phân tích tấn công → cần kiến thức để đọc packet.

---

## 5. Các khái niệm quan trọng
- **Packet List Pane**: hiển thị danh sách gói tin.
- **Packet Details Pane**: hiển thị chi tiết protocol layer.
- **Packet Bytes Pane**: dữ liệu thô (hex + ASCII).
- **Stream Follow**: ghép lại các session (TCP/UDP/HTTP).

---

## 6. Các loại Filter
### Capture Filter (dùng BPF syntax)
- Bắt ngay từ đầu, ví dụ:
  - `tcp port 80` → chỉ bắt gói TCP port 80.
  - `host 192.168.1.1` → chỉ bắt traffic với host cụ thể.

### Display Filter (mạnh mẽ hơn)
- Dùng để phân tích sau khi capture:
  - `http` → lọc gói HTTP.
  - `ip.addr == 192.168.1.5` → gói liên quan đến host.
  - `tcp.flags.syn == 1` → gói SYN.

---

## 7. Use Cases điển hình
- 🕵️‍♂️ Điều tra **traffic bất thường** (malware, C2 traffic).
- 🐞 Debug **kết nối ứng dụng** (VD: lỗi HTTP 500, TLS handshake fail).
- 🔐 Phân tích **tấn công mạng** (DoS, brute-force, credential leak).
- 🎧 Capture **VoIP/Video streaming** để phân tích chất lượng.

---

## 8. Các tool liên quan
- **tshark**: CLI version.
- **tcpdump**: capture nhanh, xuất file cho Wireshark phân tích.
- **Brim, Zeek**: thay thế/đi kèm trong phân tích nâng cao.

---

## 9. Quick Tips
- Luôn dùng **Display Filter** để phân tích, tránh ngập packet.
- Dùng `Statistics > Protocol Hierarchy` để nắm tổng quan.
- Check `Follow TCP/UDP Stream` để xem full conversation.
- Export Objects → HTTP/SMB để lấy file/malware từ traffic.
