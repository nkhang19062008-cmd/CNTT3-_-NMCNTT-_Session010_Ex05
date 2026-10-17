

## PHẦN 1. PHÂN TÍCH HỆ THỐNG TRANG TRẠI LAN
-   **Input:** Dữ liệu cảm biến (Nhiệt độ, độ ẩm không khí, độ ẩm đất, ánh sáng, EC, pH) và lệnh điều khiển từ ứng dụng.   
-   **Output:** Tín hiệu đóng ngắt rơ-le (Bơm phun sương, quạt thông gió, đèn) và thông tin hiển thị trên giao diện.   
-   **Sơ đồ phần cứng:** `[Cảm biến]` -> `[Gateway]` -> `[Cloud Server / SoC]` -> `[App / Web]` -> `[Thiết bị chấp hành]`
## PHẦN 2 & 3. NỘI DUNG FILE README.MD VÀ KIỂM THỬ SỰ CỐ
# Smart-Farm IoT - Hệ Thống Trang Trại Lan
_Hệ thống IoT giúp tự động hóa tưới tiêu, theo dõi môi trường và cảnh báo kịp thời cho trang trại Lan._
### Tính năng hệ thống
-   [x] Đăng nhập tài khoản phân quyền.  
-   [x] Giám sát thông số môi trường theo thời gian thực (Nhiệt độ, độ ẩm, EC, pH).    
-   [x] Nhận cảnh báo tự động khi thông số vượt ngưỡng  
-   [x] Điều khiển thiết bị tưới tự động hoặc thủ công trên Ap  
-   [ ] Báo cáo và xuất lịch sử dữ liệu theo ngày/tuần/tháng.    
### Hướng dẫn cài đặt
Bash
```
sudo apt update && sudo apt upgrade -y
git clone https://github.com/smartfarm/orchid-system.git
cd orchid-system
pip3 install -r requirements.txt
python3 main.py --mode=production
```
### Bảng thông số phần cứng
**Thiết bị**
**Model**
**Chức năng**
**Cảm biến**
DHT22 / Soil Moisture
Đo nhiệt độ, độ ẩm không khí và đất
**SoC**
Raspberry Pi 4
Vi điều khiển trung tâm xử lý dữ liệu
**Chấp hành**
Relay Module 12V
Đóng/mở nguồn máy bơm và quạt gió
### Chu kỳ CPU (Fetch - Decode - Execute) khi tưới Lan
> **Cơ chế xử lý lệnh bật bơm của CPU:**
> 
> -   **1. Fetch:** CPU lấy mã lệnh bật bơm từ RAM dựa vào địa chỉ ở thanh ghi `PC`.  
> -   **2. Decode:** Bộ điều khiển `CU` giải mã lệnh, xác định cần xuất tín hiệu ra chân GPIO 4. 
> -   **3. Execute:** Đơn vị `ALU` xuất điện áp ra chân GPIO 4 để đóng rơ-le bật máy bơm.    
### Mô hình DIKW trong tưới cây tự động
1.  **Data:** Cảm biến gửi về `38°C` và `Độ ẩm 50%`.    
2.  **Information:** Nhiệt độ vượt ngưỡng safe (35°C), môi trường bị khô nóng.    
3.  **Knowledge:** Vườn Lan bị nóng khô lâu sẽ dễ cháy lá và hỏng rễ.    
4.  **Wisdom:** Hệ thống tự bật máy bơm phun sương 10 phút và chạy quạt gió làm mát.  
### Lưu ý & Xử lý sự cố (Tràn RAM do tệp Log)
> **Cách xử lý an toàn khi file log phình to gây đầy RAM/trễ lệnh tưới:** 
> 1.  Tạm dừng service log: `sudo systemctl stop smartfarm-logger`     
> 2.  Xóa sạch dung lượng file log đệm: `sudo truncate -s 0 /var/log/smartfarm/system.log`     
> 3.  Khởi động lại service: `sudo systemctl start smartfarm-logger`
