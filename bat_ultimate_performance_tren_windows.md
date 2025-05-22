## **Ultimate Performance là gì?**
Là chế độ điện năng tối đa của Windows, giúp máy chạy hết công suất bằng cách tắt hầu hết các tính năng tiết kiệm điện. Phù hợp khi bạn cắm sạc và cần hiệu năng cao (lập trình nặng, render, máy ảo...). Không khuyến khích dùng khi chạy pin do nóng và hao pin cực nhanh.

---

## **Cách bật chế độ Ultimate Performance**

### **Bước 1: Mở Command Prompt hoặc PowerShell với quyền Admin**
- Nhấn `Win + X` → chọn **Windows PowerShell (Admin)** hoặc **Command Prompt (Admin)**

### **Bước 2: Chạy lệnh sau**
```bash
powercfg -duplicatescheme e9a42b02-d5df-448d-aa00-03f14749eb61
```

### **Bước 3: Kích hoạt chế độ**
- Nhấn `Win + R` 
- → gõ
```
powercfg.cpl
```
- → Enter
- Chọn **Ultimate Performance** trong danh sách.

---

## **Lưu ý**
- **Luôn cắm sạc** khi dùng.
- **Theo dõi nhiệt độ** máy nếu dùng lâu (dùng HWMonitor, HWiNFO...).
- **Không nên dùng thường xuyên trên laptop**, chỉ dùng khi cần hiệu năng cao.

---

Nếu muốn xóa chế độ này:
1. Chuyển sang chế độ khác.
2. Mở PowerShell/Command Prompt (Admin).
3. Gõ `powercfg /LIST` để lấy GUID của Ultimate Performance.
4. Chạy: `powercfg -delete [GUID đó]`.