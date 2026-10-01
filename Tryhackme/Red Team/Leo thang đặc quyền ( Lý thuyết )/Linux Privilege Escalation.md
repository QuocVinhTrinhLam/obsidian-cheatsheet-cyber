[[Window Privilege Escalation]]

---

### Privilege Escalation trên Linux

#### **Đặc điểm chung**

- Trên Linux, mục tiêu là nâng quyền từ người dùng thông thường lên **root** hoặc tài khoản có đặc quyền cao.
- Khai thác thường tập trung vào cấu hình sai, quyền tệp/thư mục, hoặc lỗ hổng kernel/dịch vụ.

#### **Các điểm cần chú ý**

1. **Khai thác quyền tệp/thư mục:**
    - **SUID/SGID bits:** Tìm các tệp thực thi có bit SUID (-rwsr-xr-x) cho phép chạy với quyền của chủ sở hữu (thường là root). Ví dụ: find / -perm -u=s -type f 2>/dev/null.
    - **Tệp cấu hình có quyền ghi:** Kiểm tra /etc/passwd, /etc/shadow hoặc các tệp cron job có quyền ghi.
    - **Thư mục nhạy cảm:** Các thư mục như /tmp hoặc /var/tmp có thể bị khai thác để chèn mã độc.
2. **Khai thác dịch vụ và cron job:**
    - **Cron job với quyền root:** Nếu cron job chạy với quyền root và sử dụng tệp người dùng có thể chỉnh sửa, kẻ tấn công có thể chèn mã độc.
    - **Dịch vụ yếu:** Dịch vụ chạy với quyền root nhưng có lỗ hổng (như phiên bản cũ của Apache, MySQL).
3. **Lỗ hổng kernel và phần mềm:**
    - **Kernel exploits:** Sử dụng các lỗ hổng kernel (như Dirty COW, CVE-2021-4034) để nâng quyền.
    - **Phần mềm lỗi thời:** Khai thác các ứng dụng như sudo, vim, hoặc screen có lỗ hổng.
4. **Sudo và thông tin xác thực:**
    - **Quyền sudo không hạn chế:** Kiểm tra lệnh sudo -l để tìm các lệnh có thể chạy với quyền root mà không cần mật khẩu.
    - **Mật khẩu lưu trữ:** Tìm mật khẩu trong lịch sử lệnh (~/.bash_history), tệp cấu hình, hoặc bộ nhớ.
5. **Công cụ phổ biến:**
    - **LinPEAS:** Kiểm tra tự động cấu hình sai và điểm yếu.
    - **GTFOBins:** Hướng dẫn khai thác các lệnh SUID/Sudo.
    - **Exploit-DB:** Tìm kiếm exploit cho kernel/phần mềm.
6. **Phòng chống:**
    - Giới hạn quyền SUID/SGID, kiểm tra thường xuyên bằng lệnh find.
    - Cập nhật kernel và phần mềm định kỳ.
    - Sử dụng AppArmor/SELinux để hạn chế quyền dịch vụ.
    - Giám sát log hệ thống (/var/log) để phát hiện hoạt động bất thường.