### **1. Cài các gói cần thiết trên Linux**

> Cập nhật hệ thống và cài thư viện USB:

```bash
sudo apt update && sudo apt install curl libimobiledevice-utils -y
```

---

### **2. Cài đặt công cụ Palera1n**

> Tải script và cài Palera1n:

```bash
sudo /bin/sh -c "$(curl -fsSL https://static.palera.in/scripts/install.sh)"
```

---

### **3. Chuẩn bị môi trường DFU**

> Dừng dịch vụ USB mặc định:

```bash
sudo systemctl stop usbmuxd
```

> Khởi chạy dịch vụ usbmuxd thủ công (Giữ Terminal này mở):

```bash
sudo usbmuxd -f -p
```

---

### **4. Jailbreak FULL ROOT lần đầu (tạo fakeFS)**

> Chạy jailbreak với fakeFS (yêu cầu 15GB trống trên iPhone):

```bash
sudo palera1n -cf
```

> Khi được yêu cầu: vào **DFU Mode** như sau:
> **Giữ nút nguồn + giảm âm lượng 10s → thả nguồn, giữ giảm âm thêm 5s.**

---

### **5. Jailbreak lại sau mỗi lần reboot iPhone**

> Dùng lệnh sau để vào lại chế độ jailbreak:

```bash
sudo palera1n -f
```

---

### **6. Gỡ bỏ jailbreak hoàn toàn (nếu cần)**

> Xóa toàn bộ dấu vết jailbreak và fakeFS:

```bash
sudo palera1n --force-revert
```

> Nếu vẫn còn fakeFS, dùng thêm:

```bash
sudo palera1n -C
```

---

**Lưu ý nhanh:**

* Cần iPhone **kết nối bằng cáp USB-A**.
* Không dùng máy ảo, ưu tiên máy **CPU Intel**.
