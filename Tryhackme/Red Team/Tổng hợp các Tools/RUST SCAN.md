## 🧠 **Cơ bản**

```bash
rustscan -a <target>
```

- `-a`: IP hoặc domain cần quét  
    **Ví dụ:**
    

```bash
rustscan -a 10.10.10.10
```

---

## ⚡ **Quét và truyền port mở cho Nmap**

bash


`rustscan -a <target> -- -A -sC -sV`

- `--`: phần sau sẽ truyền cho Nmap
    
- `-A`: quét aggressive (nmap)
    
- `-sC`: script scan (nmap)
    
- `-sV`: xác định service/version (nmap)
    

**Ví dụ:**

bash

`rustscan -a 10.10.10.10 -- -A -sC -sV`

---

## 🔍 **Tùy chọn nâng cao**

|Tham số|Ý nghĩa|
|---|---|
|`-r 1-65535`|Quét toàn bộ các port|
|`-p 22,80,443`|Quét port cụ thể|
|`--ulimit <n>`|Tăng giới hạn file descriptor (mặc định là 5000)|
|`-b 500`|Batch size: số lượng port quét song song|
|`--range`|Dùng để chỉ định dải IP|

**Ví dụ:**

bash

`rustscan -a 192.168.1.1 -r 1-1000`

---

## 💾 **Lưu kết quả**

bash

`rustscan -a 10.10.10.10 -r 1-1000 -oG result.txt`

- `-oG`: Xuất kết quả ở định dạng Grepable
    

---

## 🧪 **Chạy lệnh tùy chỉnh sau khi tìm port mở**

bash

`rustscan -a 10.10.10.10 -p 80,443 --script "curl http://{}"`

- `{}` sẽ được thay bằng IP và port
    

---

## 🔥 **Tăng tốc độ tối đa**

bash

`rustscan -a 10.10.10.10 --ulimit 10000 -b 1000`