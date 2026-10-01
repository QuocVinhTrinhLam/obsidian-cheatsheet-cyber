
---

# linPEAS 

> **Mục tiêu:** Script tự động phân tích môi trường Linux để tìm các điểm khả nghi có thể dẫn tới **nâng quyền** (Privilege Escalation).

## TL;DR

- linPEAS quét nhanh các cấu hình, quyền file, cron, sudo, kernel, service, credential lộ, và các misconfiguration thường thấy trên Linux/Unix/macOS.
    
- Dùng ngay sau khi có shell quyền thấp để tìm candidate cho privesc. Mọi phát hiện cần **xác minh thủ công** trước khi khai thác.
    

## Tải & chạy nhanh

```bash
# Tải bản phát hành chính thức
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh -o linpeas.sh
chmod +x linpeas.sh

# Chạy toàn bộ checks (đầy đủ, lâu hơn)
./linpeas.sh -a > linpeas.out

# Chạy chế độ nhanh/ít noisy
./linpeas.sh -s > linpeas_fast.out

# Chạy trực tiếp qua pipe (không lưu file trên target)
curl -sL https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | sh
```

> Lưu ý: Một số môi trường production/EDR sẽ phát hiện hành vi này — cân nhắc OPSEC.

## Các option quan trọng

- `-a` — all (quét sâu, toàn bộ checks).
    
- `-s` — stealth / fast (bỏ các checks quá noisy).
    
- `sudo ./linpeas.sh` — chạy với quyền cao nếu có (phát hiện artefact root-only).
    
- `-h` hoặc `--help` — xem hướng dẫn (tùy phiên bản).
    

## Quy trình gợi ý (Workflow)

1. Có low‑priv shell → chạy `./linpeas.sh -a > linpeas.out` (hoặc pipe về máy của bạn để tránh lưu artifact trên target).
    
2. Triage output theo priority (SUID, sudo, cron, writable files, credentials, kernel).
    
3. Xác minh từng phát hiện bằng các lệnh thủ công (ví dụ: `sudo -l`, `ls -la`, `cat` nội dung file, thử exploit an toàn).
    
4. Ghi nhận, ưu tiên theo ease-of-exploit và business impact.
    

## Lọc & triage nhanh

```bash
# Những dòng quan trọng
grep -Ei "suid|sudo|cron|writable|key|password|shadow|root|kernel" linpeas.out

# Danh sách SUID
grep -Ei "SUID" -n linpeas.out
```

## Những phát hiện ưu tiên (Actionable leads)

1. **SUID binaries** có khả năng spawn shell — kiểm tra GTFOBins và thử PoC an toàn.
    
2. **Sudo NOPASSWD** — nếu user có thể chạy lệnh dưới quyền root không cần mật khẩu → ưu tiên cao.
    
3. **Cron jobs / scripts có thể ghi đè** bởi non-root user — có thể chèn payload để chạy dưới root.
    
4. **Credentials lưu trong file** (ví dụ: `.aws/credentials`, config PHP, file `.git` chứa secrets).
    
5. **Kernel / package lỗi thời** — dò CVE tương ứng và cân nhắc exploit nếu khả thi.
    
6. **World‑writable files/dirs** thuộc resources của root hoặc services.
    

## OPSEC / Lưu ý triển khai

- Tránh lưu file lớn trên target; prefer pipe output về máy điều khiển.
    
- Chạy `-s` để giảm noise trong môi trường nhạy cảm.
    
- LinPEAS có thể báo false‑positive — **luôn** xác minh thủ công.
    

## Blue Team — Phát hiện & Hardening

- **Phát hiện:** spike về process `curl|wget|sh` từ user không thường dùng, truy vấn đọc `/etc/shadow`, exec từ `/tmp` hoặc `/dev/shm`.
    
- **Hardening:** loại bỏ SUID không cần thiết, áp dụng `noexec,nosuid` cho /tmp, rà soát sudoers, bật auditd để log `execve` và truy vấn file nhạy cảm.
    
- **Playbook phản ứng:** giữ snapshot process, phân tích file descriptor `/proc/<pid>/fd`, tìm các artifact script, và điều tra nguồn outbound (địa chỉ IP / domain).
    

## Công cụ bổ trợ

- `pspy`, `linenum`, `lse` (linux-smart-enumeration), GTFOBins
    
- Repo chính: [https://github.com/carlospolop/PEASS-ng](https://github.com/carlospolop/PEASS-ng)
    

---
