## 1. Chuẩn bị môi trường

Mở terminal, cập nhật repo và cài thư viện cần thiết:

```bash
sudo apt update
sudo apt install libaio1 libncurses5
```

Kiểm tra phiên bản glibc:

```bash
ldd --version
```

(Phải từ 2.28 trở lên, nếu Linux Mint 22 thì khỏi lo.)

---

## 2. Tải MySQL dạng tar.xz

Vào trang:

[https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/)

* OS: Linux - Generic
* OS Version: glibc 2.28 (x86, 64-bit)
* Format: Compressed TAR Archive (.tar.xz)

Tải về ví dụ file: `mysql-8.0.36-linux-glibc2.28-x86_64.tar.xz`

---

## 3. Giải nén và di chuyển

Chạy lệnh:

```bash
cd ~/Downloads
tar -xvf mysql-8.0.36-linux-glibc2.28-x86_64.tar.xz
sudo mv mysql-8.0.36-linux-glibc2.28-x86_64 /opt/mysql
```

---

## 4. Tạo user & group mysql, phân quyền

```bash
sudo groupadd mysql
sudo useradd -r -g mysql -s /bin/false mysql
sudo chown -R mysql:mysql /opt/mysql
sudo mkdir /opt/mysql/data
sudo chown mysql:mysql /opt/mysql/data
```

---

## 5. Khởi tạo database

Chạy lệnh:

```bash
cd /opt/mysql
sudo bin/mysqld --initialize --user=mysql --basedir=/opt/mysql --datadir=/opt/mysql/data
```

Kết quả sẽ in mật khẩu root tạm thời, nhớ copy lại!

---

## 6. Tạo file cấu hình my.cnf

Tạo file:

```bash
sudo nano /etc/my.cnf
```

Dán vào:

```ini
[mysqld]
basedir=/opt/mysql
datadir=/opt/mysql/data
port=3306
socket=/tmp/mysql.sock
symbolic-links=0
skip-external-locking
```

Lưu và thoát.

---

## 7. Tạo systemd service để quản lý MySQL

Tạo file:

```bash
sudo nano /etc/systemd/system/mysql.service
```

Dán vào:

```ini
[Unit]
Description=MySQL Server
After=network.target

[Service]
Type=forking
ExecStart=/opt/mysql/bin/mysqld_safe --datadir=/opt/mysql/data
ExecStop=/opt/mysql/bin/mysqladmin shutdown
User=mysql
Group=mysql
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Lưu và thoát.

---

## 8. Khởi động MySQL service

```bash
sudo systemctl daemon-reload
sudo systemctl enable mysql
sudo systemctl start mysql
sudo systemctl status mysql
```

Nếu status hiện **active (running)**, thì MySQL đã sẵn sàng.

---

## 9. Đổi mật khẩu root

Đăng nhập:

```bash
/opt/mysql/bin/mysql -u root -p
```

Nhập mật khẩu tạm thời đã copy ở bước 5, sau đó chạy:

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MatKhauMoiCucManh!';
FLUSH PRIVILEGES;
EXIT;
```

---

## 10. Tạo user kết nối cho DBeaver

```bash
/opt/mysql/bin/mysql -u root -p
```

Sau khi nhập mật khẩu mới, chạy:

```sql
CREATE USER 'dbeaver_user'@'%' IDENTIFIED BY 'MatKhauDBeaver';
GRANT ALL PRIVILEGES ON *.* TO 'dbeaver_user'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```

---

## 11. (Tùy chọn) Cho phép kết nối từ xa

Mở file `/etc/my.cnf`:

```bash
sudo nano /etc/my.cnf
```

Thêm hoặc chỉnh dòng:

```ini
bind-address = 0.0.0.0
```

Lưu và restart mysql:

```bash
sudo systemctl restart mysql
```

Nếu bật firewall, mở cổng 3306:

```bash
sudo ufw allow 3306
```

---

## 12. Kết nối DBeaver

* Mở DBeaver trên máy của bạn.
* Tạo mới kết nối: bấm **New Database Connection** > chọn **MySQL**.
* Nhập thông tin:

  * Host: IP máy Linux (hoặc `127.0.0.1` nếu cùng máy)
  * Port: `3306`
  * Database: để trống hoặc `mysql`
  * Username: `dbeaver_user`
  * Password: `MatKhauDBeaver`
* Bấm **Test Connection**. Nếu chưa có driver, đồng ý tải.
* Nếu thành công, bấm **Finish**.

---

## 13. Bonus: Thêm MySQL vào PATH để dùng tiện hơn

```bash
echo 'export PATH=$PATH:/opt/mysql/bin' >> ~/.bashrc
source ~/.bashrc
```

---