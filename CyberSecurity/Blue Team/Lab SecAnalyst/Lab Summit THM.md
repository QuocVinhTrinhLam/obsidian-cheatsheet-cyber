[Tryhackme/Summit](https://tryhackme.com/room/summit)

## **Sample 1**
![[Screenshot 2025-08-12 at 12.02.04.png]]
![[Screenshot 2025-08-12 at 12.03.25.png]]
Sau khi quét phát hiện Hash MD5 - Tiến hành submit hash
![[Screenshot 2025-08-12 at 12.04.01.png]]

___
## **Sample 2**
![[Screenshot 2025-08-12 at 12.05.13.png]]
Phát hiện ip/port lạ được kết nối vào - Tiến hành quản lý firewall
![[Screenshot 2025-08-12 at 12.06.37.png]]
Type : Egress
Source IP : any
Destination IP : 154.35.10.113
Action : Deny
Rule này sẽ **ngăn tất cả thiết bị bên trong mạng gửi dữ liệu ra ngoài tới IP 154.35.10.113**, bất kể cổng nào hoặc giao thức nào.  
Điều này có thể được dùng để:

- Chặn kết nối tới một máy chủ độc hại.
    
- Ngăn dữ liệu rò rỉ ra một địa chỉ IP nghi ngờ.

---
## **Sample 3**
![[Screenshot 2025-08-12 at 12.09.46.png]]
Download file chứa mã độc `backdoor.exe` từ internet - Tiến hành ngăn chặn DNS
![[Screenshot 2025-08-12 at 12.11.41.png]]
![[Screenshot 2025-08-12 at 12.11.53.png]]
___
## **Sample 4**
![[Screenshot 2025-08-12 at 12.13.09.png]]
Process đầu nghi ngờ vì `DisableRealtimeMonitoring` 
![[Screenshot 2025-08-12 at 12.18.07.png]]
___
## **Sample 5**
![[Screenshot 2025-08-12 at 12.19.35.png]]
Download và Execute file từ internet `beacon.bat`
![[Screenshot 2025-08-12 at 12.24.19.png]]
___
## **Sample 6**
![[Screenshot 2025-08-12 at 12.25.47.png]]
Lệnh được execute và lưu vào `%temp%`
![[Screenshot 2025-08-12 at 12.26.39.png]]
