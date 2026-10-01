
___

## TL;DR

- enum4linux là một wrapper (Perl) gọi `rpcclient`, `smbclient`, `nmblookup`... để thu thập nhiều thông tin từ host Windows qua SMB (cổng 445/139).
    
- Thích hợp cho reconnaissance trong mạng nội bộ sau khi phát hiện SMB mở.
    

## Cài đặt & bản nâng cấp

- Trên Kali thường đã cài sẵn `enum4linux`. Nếu muốn tính năng SMB2/3 và JSON output, cân nhắc `enum4linux-ng` (Python).
    

## Cú pháp cơ bản

```bash
enum4linux [options] <target-ip>
```

## Tùy chọn quan trọng

- `-a` — run all checks (quét toàn diện).
    
- `-U` — enumerate users.
    
- `-S` — enumerate shares.
    
- `-G` — enumerate groups.
    
- `-P` — password policy checks.
    
- `-r` — RID range enumeration.
    
- `-u <user> -p <pass>` — dùng credential để chạy authenticated checks.
    
- `-d` — debug/verbose.
    

## Ví dụ vận hành

```bash
# Quét toàn bộ
enum4linux -a 10.10.10.5

# Chỉ liệt kê users
enum4linux -U 192.168.1.50

# Có credentials
enum4linux -u 'svc_user' -p 'P@ssw0rd' -a 10.0.0.12
```

## Kết hợp vào workflow

1. Dùng `nmap -p 139,445 --open -sV` để tìm host có SMB mở.
    
2. Chạy `enum4linux -a` để thu thập user/group/share/OS.
    
3. Dùng kết quả để tiếp tục với `smbclient`, `smbmap`, `crackmapexec`, hoặc password spraying/brute-force.
    

## Những phát hiện phổ biến & hành động tương ứng

- **Shares list** — kiểm tra share writable, backup, hoặc file cấu hình chứa mật khẩu.
    
- **User list** — tạo wordlist username cho password spraying.
    
- **Domain/Workgroup & SID** — hỗ trợ phân tích cấu trúc mạng và chính sách.
    
- **Password policy** — tối ưu chiến lược brute-force (độ dài, complexity, lockout).
    

## Hạn chế & giải pháp thay thế

- SMB signing, firewall, hoặc SMBv1 bị disable có thể hạn chế dữ liệu trả về.
    
- Thay thế/bổ sung: `smbmap`, `crackmapexec`, `smbclient`, `enum4linux-ng`.
    

## Phòng thủ (Defensive Guidance)

- Giám sát các hành vi enumerate SMB/RPC bất thường (số lượng RPC calls, enumerate shares nhiều lần).
    
- Áp dụng SMB signing, segment hóa mạng, giới hạn truy cập SMB theo whitelist.
    
- Giới hạn account với quyền ngang hàng và theo dõi pattern credential harvesting.
    

## Lệnh thao tác nhanh

```bash
# Lọc các dòng quan trọng
enum4linux -a 10.10.10.5 | egrep -i "user|group|share|domain|policy|sid"

# Thử bản Python fork
git clone https://github.com/cddmp/enum4linux-ng.git
python3 enum4linux-ng.py -A 10.10.10.5
```

---
