
---
![[Screenshot 2025-09-30 at 20.51.05.png]]
## ✅ **1. Kiểm tra trạng thái & thông tin**

```bash
git status             # Xem trạng thái file (thay đổi, staged, untracked)
git log                # Xem lịch sử commit
git log --oneline      # Lịch sử commit ngắn gọn
git diff               # Xem chi tiết thay đổi chưa staged
git diff --staged      # Xem thay đổi đã staged
```

---

## ✅ **2. Thêm & Commit**

```bash
git add <file>         # Thêm file cụ thể
git add .              # Thêm tất cả thay đổi
git commit -m "msg"    # Commit với message ngắn gọn
git commit -am "msg"   # Add + commit file đã tracked
```

---

## ✅ **3. Push & Pull**

```bash
git push origin main   # Đẩy commit lên branch main
git push -u origin main # Liên kết nhánh local với remote
git pull origin main   # Kéo code mới từ remote
git pull --rebase      # Kéo code và giữ lịch sử commit gọn
```

---

## ✅ **4. Branch**

```bash
git branch             # Liệt kê nhánh
git branch <name>      # Tạo nhánh mới
git checkout <name>    # Chuyển sang nhánh
git checkout -b <name> # Tạo + chuyển sang nhánh
git merge <branch>     # Merge nhánh vào current
```

---

## ✅ **5. Undo & Reset**

```bash
git restore <file>     # Hủy thay đổi chưa staged
git reset <file>       # Gỡ khỏi staged (nhưng giữ nội dung)
git reset --hard       # Reset toàn bộ về commit trước (mất thay đổi)
git revert <hash>      # Tạo commit mới để đảo ngược commit trước
```

---

## ✅ **6. Remote**

```bash
git remote -v          # Kiểm tra remote
git remote add origin <url> # Thêm remote repo
git clone <url>        # Clone repo từ GitHub
```

---

## ✅ **7. Kiểm tra commit**

```bash
git show <hash>        # Xem chi tiết commit
git blame <file>       # Ai sửa dòng nào
```

---

🔥 **Pro Tips cho Workflow cá nhân**:

- Luôn `git pull --rebase` trước khi push để tránh conflict.
    
- Commit message nên rõ ràng:
    
    - `feat:` Thêm tính năng
        
    - `fix:` Sửa bug
        
    - `docs:` Cập nhật docs
        
    - `style:` Format, UI
        

---

