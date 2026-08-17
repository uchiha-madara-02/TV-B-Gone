# 🛰️ TV-B-GONE [ESP32 / ESP8266]

Dự án này hướng dẫn cách kết nối và sử dụng các dòng vi điều khiển (ESP32-S3, ESP32-C3, ESP8266) để điều khiển một đèn LED hồng ngoại (IR) công suất cao. Hệ thống sử dụng Transistor 2N2222 làm mạch đệm khuếch đại dòng điện và một nút nhấn để điều khiển linh hoạt việc phát tín hiệu.

---

## 🛠️ Danh Sách Linh Kiện 

Để thực hiện dự án, bạn cần chuẩn bị các linh kiện sau:
* **Vi điều khiển:** ESP32-S3, ESP32-C3, hoặc NodeMCU ESP8266.
* **Transistor NPN:** 2N2222 (Đóng vai trò công tắc điện tử).
* **Đèn LED:** LED IR (Hồng ngoại) loại 1.5V.
* **Điện trở:** * 1 x 100 Ω (Bảo vệ cực Base của Transistor).
  * 1 x 10.0 Ω (Hạn dòng bảo vệ LED IR).
* **Nút nhấn:** 1 x Push button (Dùng để điều khiển trạng thái).

---

## 🖼️ Sơ Đồ Đấu Nối & Mô Tả Chi Tiết

Dưới đây là hướng dẫn đấu nối cụ thể cho từng loại board mạch mà bạn có thể sử dụng.

### 1. Phiên bản sử dụng ESP32-S3
|<img src="./Screenshot 2025-12-25 231053.png" width="450">|

**Mô tả kết nối cho ESP32-S3:**
* **Chân phát tín hiệu:** Khối vi điều khiển sử dụng chân **GPIO 9** (chân màu xanh đậm trên sơ đồ). Chân này xuất tín hiệu, đi qua điện trở bảo vệ **100Ω** và kết nối vào chân giữa (chân Base) của Transistor 2N2222.
* **Mạch công suất LED:** Chân dương của LED IR lấy nguồn từ chân **3.3V / 5V** của board mạch thông qua một điện trở hạn dòng **10.0Ω**. Chân âm của LED nối vào cực Phải (Collector) của Transistor. Cực Trái (Emitter) của Transistor nối về **GND**.

### 2. Phiên bản sử dụng ESP32-C3
|<img src="./Screenshot 2025-12-25 231016.png" width="450">|

**Mô tả kết nối cho ESP32-C3:**
* **Chân phát tín hiệu:** Đối với board ESP32-C3, tín hiệu điều khiển được xuất ra từ chân **GPIO 21** (chân màu xanh lá trên sơ đồ). Tương tự như trên, tín hiệu này đi qua điện trở **100Ω** để kích hoạt chân Base của Transistor 2N2222.
* **Mạch công suất LED:** Giữ nguyên nguyên lý nối mạch tải. Cấp nguồn **3.3V / 5V** qua điện trở **10.0Ω** vào cực dương LED. Cực âm LED nối vào chân Collector của Transistor. Chân Emitter của Transistor nối với **GND** để hoàn thành mạch kín.

### 3. Phiên bản sử dụng ESP8266 (Kèm Nút Nhấn)
|<img src="./Screenshot 2025-12-25 230710.png" width="450">|

**Mô tả kết nối cho ESP8266 & Nút nhấn:**
* **Chân phát tín hiệu:** Mạch sử dụng chân **D5 (GPIO 14)** (dây màu xanh dương) để xuất tín hiệu IR. Tín hiệu qua điện trở **100Ω** vào chân Base của Transistor 2N2222. Mạch tải của LED IR lắp tương tự như hai phiên bản trên.
* **Chân điều khiển (Nút nhấn):** Nút nhấn được đấu vào chân **D6 (GPIO 12)** (dây màu vàng). Một đầu của nút nhấn nối với chân D6, đầu còn lại nối thẳng vào **GND** (dây màu đen). Khi lập trình, chân D6 sẽ được cấu hình ở chế độ `INPUT_PULLUP`.

---

## ⚙️ Logic Hoạt Động Của Hệ Thống

Hệ thống được lập trình để phản hồi lại các thao tác từ người dùng thông qua nút nhấn (như được thể hiện trong sơ đồ thứ 3):

* **Nhấn 1 lần (Single Click):** * Bắt đầu gửi mã IR. 
  * Nếu hệ thống đang trong quá trình gửi mã mà bạn nhấn 1 lần nữa, hệ thống sẽ **Tạm dừng (Pause)** việc gửi mã (thời gian gửi hết mã của bản cũ là 15p, bản mới (v3) là 5p).
* **Nhấn 2 lần (Double Click):** * Bất kể hệ thống đang ở trạng thái nào (đang gửi hoặc đang tạm dừng), thao tác này sẽ ép hệ thống **Bắt đầu gửi lại mã từ đầu (Restart)**.

---

## ⚠️ Lưu Ý Kỹ Thuật
1. **Xác định đúng chân Transistor 2N2222:** Đặt mặt phẳng (mặt có chữ) của Transistor hướng về phía bạn, 3 chân từ trái qua phải lần lượt là **E (Emitter) - B (Base) - C (Collector)**. Nếu đấu ngược có thể làm hỏng linh kiện hoặc mạch không hoạt động.
2. **Không bỏ qua điện trở:** Bắt buộc phải có điện trở 10.0Ω nối tiếp với LED IR. Nếu cấp nguồn trực tiếp qua Transistor mà không có trở hạn dòng, LED IR có thể bị cháy ngay lập tức.
