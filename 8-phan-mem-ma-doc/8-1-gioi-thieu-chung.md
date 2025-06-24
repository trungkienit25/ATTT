# Bài 8: Phần mềm mã độc (Malware)

### 1. Giới thiệu chung

#### 1.1. Khái niệm

* **Phần mềm độc hại (Malicious Software hay Malware):** Là những chương trình máy tính mà khi thực thi sẽ gây tổn hại tới tài nguyên của hệ thống hoặc chiếm đoạt một phần/toàn bộ quyền điều khiển hệ thống.
* **Phân loại:** Malware có thể được phân thành ba loại chính, mặc dù sự phân biệt này trong thực tế đôi khi không rõ ràng.
    * **Virus:** Cần hành động kích hoạt của người dùng để lây nhiễm.
    * **Worm (Sâu máy tính):** Không cần hành động kích hoạt của người dùng để lây nhiễm mà có thể tự lây lan qua mạng.
    * **Trojan:** Là một chương trình độc hại ẩn giấu trong các tệp tin có vẻ vô hại và không có khả năng tự lây nhiễm.

#### 1.2. Các hành vi gây hại

Phần mềm mã độc có thể thực hiện nhiều hành vi phá hoại khác nhau, có thể được kích hoạt ngay lập tức hoặc chờ đợi một điều kiện nào đó (time bomb, logic bomb).

* Phá hủy dữ liệu, phần cứng.
* Nghe trộm hoạt động của người dùng trên các thiết bị vào ra (Keylogging).
* Đánh cắp thông tin (Spyware).
* Mã hóa dữ liệu để đòi tiền chuộc (Ransomware).
* Đánh cắp tài nguyên tính toán để đào tiền ảo (Coinminer).
* Tạo cửa hậu (Backdoor) để kẻ tấn công có thể xâm nhập và điều khiển từ xa.
* Che giấu hoạt động của chính nó và của kẻ tấn công (Rootkit).
* Biến máy tính nạn nhân thành một phần của mạng máy tính ma (Botnet) để thực hiện các cuộc tấn công khác.

#### 1.3. Các con đường lây truyền

Malware có thể lây lan qua rất nhiều con đường khác nhau:
* Email (file đính kèm hoặc đường dẫn độc hại).
* Các ứng dụng truyền thông điệp (Instant Messaging).
* Các thiết bị lưu trữ di động (USB, ổ cứng ngoài).
* Các chương trình giả mạo phần mềm hợp lệ.
* Tiện ích chia sẻ file trong mạng LAN.
* Phần mềm bẻ khóa bản quyền (crack).
* Các chương trình chia sẻ file ngang hàng (P2P).
* Khai thác các lỗ hổng của phần mềm chưa được vá.

#### 1.4. Kịch bản phát tán và lây nhiễm

* **Kịch bản 1: Lừa đảo qua Email/Đường dẫn** 
    1.  Kẻ tấn công tải malware lên một máy chủ do chúng kiểm soát.
    2.  Chúng gửi email lừa đảo cho nạn nhân, chứa một đường dẫn tới máy chủ đó.
    3.  Nạn nhân tò mò và click vào đường dẫn.
    4.  Trình duyệt của nạn nhân kết nối tới máy chủ độc hại.
    5.  Malware được tự động tải về máy tính.
    6.  Nạn nhân kích hoạt file vừa tải về, khiến máy tính bị nhiễm.

* **Kịch bản 2: Khai thác lỗ hổng tự động (Drive-by Download)** 
    1.  Nạn nhân truy cập vào một website trông có vẻ vô hại (nhưng đã bị kẻ tấn công xâm nhập).
    2.  Trang web này sẽ bí mật chuyển hướng kết nối của nạn nhân tới một máy chủ khai thác (Exploit Server).
    3.  Máy chủ khai thác sẽ do thám, thu thập thông tin về hệ điều hành và trình duyệt của nạn nhân.
    4.  Nó phát hiện ra một lỗ hổng bảo mật chưa được vá trên máy của nạn nhân.
    5.  Nó tự động khai thác lỗ hổng đó để cài đặt malware lên máy nạn nhân mà không cần bất kỳ hành động nào từ người dùng.