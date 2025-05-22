**Cheatsheet Siêu Gọn: Xuất/Nhập Driver Windows 10 (CMD)**

**Yêu cầu:** Mở **Command Prompt (CMD)** với quyền **Administrator**.

**1. Xuất Driver (Backup):**

* **Tạo thư mục lưu:** Ví dụ: `C:\DriverBackup`
* **Chạy lệnh (thay đường dẫn nếu cần):**
    ```cmd
    dism /online /export-driver /destination:"C:\DriverBackup"
    ```

**2. Nhập Driver (Restore):**

* **Chạy lệnh (thay đường dẫn nếu cần):**
    ```cmd
    pnputil /add-driver "C:\DriverBackup\*.inf" /subdirs /install
    ```

**Lưu Ý Quan Trọng:**

* Luôn chạy CMD với **quyền Administrator**.
* Đảm bảo **đường dẫn thư mục** trong lệnh là chính xác (dùng `""` nếu có khoảng trắng).
* **Kiểm tra Device Manager** sau khi nhập driver, có thể cần **khởi động lại** máy.

