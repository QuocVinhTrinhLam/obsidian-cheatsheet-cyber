[[Enumeration]] | [[PrivilegeEscalation (Enumeration)]]
[[Netstat]] | [[Nmap, Dig and NSlookup]]

# Linux 
[[Linux Privilege Escalation]]
get more information about the Linux distribution and release version by searching for files or links that end with `-release` in `/etc/`. 

```Terminal
user@TryHackMe$ ls /etc/*-release /etc/centos-release /etc/os-release /etc/redhat-release /etc/system-release 
$ cat /etc/os-release
```

![[Screenshot 2025-08-08 at 15.37.13.png]]

```Terminal
user@TryHackMe$ who 
root tty1 2022-05-18 13:24 
jane pts/0 2022-05-19 07:17 (10.20.30.105) 
peter pts/1 2022-05-19 07:13 (10.20.30.113)
```

```Terminal
user@TryHackMe$ id uid=1003(jane) gid=1003(jane) groups=1003(jane) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```

Sử dụng last để biết được ai là người last login
```Terminal
user@TryHackMe$ last 
jane pts/0 10.20.30.105 Thu May 19 07:17 still logged in 
peter pts/1 10.20.30.113 Thu May 19 07:13 still logged in 
michael pts/0 10.20.30.1 Thu May 19 05:12 - 05:17 (00:04) 
randa pts/1 10.20.30.107 Wed May 18 14:18 - 07:08 (16:49) 
root tty1 Wed May 18 13:24 still logged in 
[...]
```

![[Screenshot 2025-08-08 at 15.49.20.png]]
Tìm DNS bằng `cat /etc/resolv.conf` 

[[Netstat]]

![[Screenshot 2025-08-08 at 16.06.44.png]]

---

![[Screenshot 2025-08-08 at 16.03.01.png]]

**ps** – một lệnh trong Linux/Unix dùng để liệt kê các tiến trình (process) đang chạy trên hệ thống. Mình sẽ giải thích kỹ hơn từng phần để bạn dễ hiểu và ứng dụng trong pentest hoặc quản trị hệ thống.

---

## Mục đích của `ps`

`ps` (process status) giúp bạn:

* **Xem tiến trình nào đang chạy** (ví dụ dịch vụ web, SSH, database, malware…)
* **Kiểm tra người dùng nào khởi chạy tiến trình đó**
* **Xem PID** (Process ID) để có thể kill hoặc debug tiến trình
* **Xem thông tin như CPU/RAM sử dụng, thời gian chạy**

---

## Cú pháp phổ biến

Linux có **2 phong cách cú pháp** cho `ps`:

* **Unix/GNU style** → dùng dấu `-` trước option (vd: `ps -ef`)
* **BSD style** → không có dấu `-` (vd: `ps aux`)

---

## Các tùy chọn quan trọng

| Option | Ý nghĩa                                                        |
| ------ | -------------------------------------------------------------- |
| `-e`   | Hiển thị tất cả tiến trình trên hệ thống                       |
| `-f`   | Full-format listing (thêm cột như UID, PPID, CMD, TIME…)       |
| `-j`   | Hiển thị ở dạng job format                                     |
| `-l`   | Long format (thêm thông tin chi tiết như priority, nice value) |
| `-u`   | User-oriented format (thêm thông tin CPU, RAM, thời gian chạy) |

---

## Các lệnh thường dùng

### **Unix/GNU style**

```bash
ps -e        # Liệt kê tất cả process
ps -ef       # Liệt kê tất cả với đầy đủ thông tin
ps -el       # Liệt kê tất cả với định dạng dài (long format)
```

### **BSD style**

```bash
ps ax        # Liệt kê tất cả process
ps aux       # Liệt kê tất cả với thông tin người dùng, CPU, RAM
```

> `a` → hiển thị process của tất cả user
> `x` → hiển thị process không cần TTY (tiến trình chạy ngầm)
> `u` → thêm thông tin về user, CPU, RAM

---

## Ví dụ minh họa

```bash
ps -ef | grep apache
```

* Tìm tiến trình Apache đang chạy.

```bash
ps aux --sort=-%cpu | head
```

* Liệt kê các process tốn CPU nhiều nhất.

```bash
ps -u root
```

* Xem tất cả process đang chạy dưới quyền root.

---

## Ứng dụng thực tế trong bảo mật

* **Pentest**: Khi khai thác thành công một máy, bạn có thể dùng `ps` để xem dịch vụ nào đang chạy, từ đó tìm thêm điểm tấn công (ví dụ MySQL, ngrok, reverse shell).
* **Incident Response (Blue Team)**: Phát hiện tiến trình lạ, CPU usage cao bất thường, malware chạy ẩn (`ps aux | grep <tên>`).
* **Forensics**: Kết hợp với `lsof` và `netstat` để xác định tiến trình nào đang mở port khả nghi.

---

# Window
[[Window Privilege Escalation]]
`systeminfo` tra cứu thông tin của máy 
![[Screenshot 2025-08-08 at 16.13.41.png]]

You can check installed updates using `wmic qfe get Caption,Description`
![[Screenshot 2025-08-08 at 16.29.17.png]]

`wmic qfe get Caption,Description`. This information will give you an idea of how quickly systems are being patched and updated.
![[Screenshot 2025-08-08 at 16.31.48.png]]

You can list the users that belong to the local administrators’ group using the command `net localgroup administrators`
![[Screenshot 2025-08-08 at 16.34.28.png]]

On MS Windows, we can use `netstat` to get various information, such as which ports the system is listening on, which connections are active, and who is using them. In this example, we use the options `-a` to display all listening ports and active connections. The `-b` lets us find the binary involved in the connection, while `-n` is used to avoid resolving IP addresses and port numbers. Finally, `-o` display the process ID (PID).
![[Screenshot 2025-08-08 at 16.41.27.png]]

[[Nmap, Dig and NSlookup]]

Sử dụng dig `dig -t AXFR Domain_Name @DNS_SERVER`

![[Screenshot 2025-08-08 at 16.46.55.png]]

Sử dụng tool SNMP cung cấp quyền truy cập chia sẻ vào tệp và máy in

![[Screenshot 2025-08-08 at 16.52.54.png]]
