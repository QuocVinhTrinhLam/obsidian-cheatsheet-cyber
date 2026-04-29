### Bước 2: Cấu hình Lớp 2 (Switching - Xây dựng hạ tầng)

**1. Khai báo danh sách VLAN trên TẤT CẢ các Switch (Kể cả Switch trung tâm):**

```
Switch(config)# vlan 10
Switch(config-vlan)# name GiamDoc
! (Làm tương tự cho các VLAN 20, 30, 40, 50, 60)
```

**2. Cấu hình các đường Trunk (Đường cao tốc):**


```
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan all
```

**3. Cấu hình cổng Access (Gán nhà cho PC):**


```
Switch(config)# interface range fa0/5-6
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# spanning-tree portfast
```

---

### Bước 3: Cấu hình Lớp 3 (Routing - Bộ não điều hướng)

**1. "Đánh thức" cổng vật lý chính:**

```
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# no shutdown
```

**2. Tạo cổng ảo và cấp Gateway cho từng VLAN:**

```
Router(config)# interface gigabitEthernet 0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.1.129 255.255.255.224
! (Lặp lại cho các VLAN khác với IP Gateway tương ứng)
```

---

### Bước 4: Cấu hình Dịch vụ (DHCP Server)

**1. Loại trừ các IP không được cấp:** (Thường là IP Gateway hoặc IP của Server).

```
Router(config)# ip dhcp excluded-address 192.168.1.129
```

**2. Tạo DHCP Pool cho từng phòng ban:**

```
Router(config)# ip dhcp pool GiamDoc
Router(dhcp-config)# network 192.168.1.128 255.255.255.224
Router(dhcp-config)# default-router 192.168.1.129
Router(dhcp-config)# dns-server 8.8.8.8
```

---

### Bước 5: Kích hoạt thiết bị đầu cuối

- **Server:** Luôn luôn đặt **IP tĩnh (Static)** để đảm bảo tính ổn định của dịch vụ.
    
- **PC Nhân viên:** Vào phần cấu hình IP, chuyển sang chế độ **DHCP**. Nếu nhận được đúng dải IP và Gateway đã quy hoạch, tức là bạn đã cấu hình thành công 90% bài Lab.
    
- **Test:** Dùng lệnh Ping giữa các VLAN để đảm bảo mạng đã thông suốt toàn diện.
    

---

### Bước 6: Lập chốt kiểm soát (Cấu hình ACL)

**1. Viết luật (Access-List):**

```
! Ví dụ: Chặn mạng Khách (192.168.1.0/25) vào Server (192.168.2.0/29)
Router(config)# ip access-list extended BLOCK_GUEST
Router(config-ext-nacl)# deny ip 192.168.1.0 0.0.0.127 192.168.2.0 0.0.0.7
Router(config-ext-nacl)# permit ip any any
```

**2. Áp dụng luật vào cánh cửa (Interface):**

```
Router(config)# interface gigabitEthernet 0/0.50
Router(config-subif)# ip access-group BLOCK_GUEST in
```
