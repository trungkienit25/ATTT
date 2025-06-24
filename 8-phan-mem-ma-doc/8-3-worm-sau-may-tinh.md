### 3. Sâu máy tính (Computer Worm)

Sâu máy tính là một loại phần mềm độc hại có khả năng tự sao chép và lây lan qua các mạng máy tính.

#### 3.1. Cách thức lây lan

* **Tự lây lan:** Điểm khác biệt cốt lõi so với virus là sâu máy tính **không cần hành động kích hoạt của người dùng** để lây nhiễm.
* **Khai thác lỗ hổng:** Chúng thường lây lan bằng cách khai thác các lỗ hổng phần mềm trên các hệ thống mục tiêu.
* **Tìm kiếm nạn nhân mới:** Sau khi lây nhiễm thành công một máy, sâu sẽ bắt đầu tìm kiếm các nạn nhân tiếp theo bằng nhiều cách:
    * Quét mạng theo dải địa chỉ IP.
    * Chọn các địa chỉ IP ngẫu nhiên.
    * Sử dụng các danh sách máy tính có sẵn trong file cấu hình.
    * Truy vấn tới các máy chủ của bên thứ ba.
* **Tấn công và lây nhiễm:** Khi tìm thấy một nạn nhân tiềm năng, sâu sẽ quét để phát hiện lỗ hổng tương tự và khai thác nó để lây nhiễm.

Mô hình lây lan của sâu máy tính tương tự như sự lây lan của virus sinh học, với tốc độ lây nhiễm rất nhanh, thường nhanh hơn virus rất nhiều.

#### 3.2. Các sâu máy tính nổi tiếng trong lịch sử

* **Sâu Morris (1988):**
    * Được coi là sâu máy tính đầu tiên, ban đầu được thiết kế để đo đạc kích thước của Internet nhưng đã vô tình gây ra một cuộc tấn công từ chối dịch vụ (DoS) trên diện rộng.
    * Nó khai thác nhiều loại lỗ hổng khác nhau, bao gồm tràn bộ đệm và dò đoán mật khẩu yếu.
    * Đã lây nhiễm khoảng 10% số máy tính trên Internet vào thời điểm đó, gây thiệt hại ước tính 100 triệu đô la.
* **Sâu Code Red (2001):**
    * Khai thác một lỗ hổng của phần mềm máy chủ Web Microsoft IIS, dù bản vá đã có trước đó 1 tháng.
    * Đã lây nhiễm khoảng 300.000 máy chủ chỉ sau 13 giờ.
    * Phần mềm độc hại này có hai hành vi chính: thay đổi nội dung trang chủ với dòng chữ "Hacked By Chinese!" và kích hoạt "bom hẹn giờ" để tấn công DoS vào website của Nhà Trắng.
* **Sâu SQL Slammer (2003):**
    * Khai thác lỗ hổng của MS SQL Server.
    * Lây nhiễm tới 75.000 máy tính chỉ sau 10 phút, với số lượng máy bị nhiễm tăng gấp đôi sau mỗi 8.5 giây.
    * Đến năm 2016, sâu này đã quay trở lại và tấn công các máy chủ ở 172 quốc gia, trong đó có rất nhiều địa chỉ IP thực hiện tấn công là ở Việt Nam.
* **Conficker (2008):**
    * Khai thác các lỗ hổng trên nhiều phiên bản hệ điều hành Windows.
    * Sử dụng nhiều phương thức lây nhiễm khác nhau như khai thác lỗ hổng, lây qua mạng nội bộ và qua USB.
    * Đã xây dựng nên một trong những mạng botnet lớn nhất vào thời điểm đó, và Việt Nam nằm trong số các quốc gia có số lượng máy tính bị nhiễm cao nhất.
* **Stuxnet (2010):**
    * Là một vũ khí mạng tinh vi, được cho là nhắm vào chương trình hạt nhân của Iran.
    * Nó tấn công các máy tính trong mạng công nghiệp SCADA điều khiển máy ly tâm.
    * Stuxnet đã khai thác đến 4 lỗ hổng zero-day khác nhau và lây nhiễm ban đầu qua USB.
    * Hành vi phá hoại của nó rất tinh vi: chỉ kích hoạt khi máy ly tâm đạt một tốc độ nhất định, sau đó từ từ tăng tốc để phá hỏng máy, đồng thời gửi thông báo giả mạo về trung tâm điều khiển rằng hệ thống vẫn hoạt động bình thường.
* **WannaCry (2017):**
    * Là một loại sâu ransomware, gây ra một trong những vụ tấn công mạng có quy mô lớn nhất trong lịch sử.
    * Nó khai thác lỗ hổng **EternalBlue** trên hệ điều hành Windows bằng cách sử dụng công cụ bị rò rỉ của Cơ quan An ninh Quốc gia Hoa Kỳ (NSA).
    * Đã lây nhiễm trên 230.000 máy tính, mã hóa dữ liệu và đòi tiền chuộc, trong đó Việt Nam là một trong những nước bị ảnh hưởng nặng nề nhất.

#### 3.3. Phòng chống và giảm thiểu

Để phòng chống sâu máy tính và các loại malware khác, cần áp dụng một chiến lược bảo vệ theo chiều sâu:

* **Đối với người dùng:**
    * Tránh mở các file đính kèm hoặc đường dẫn từ các email không rõ nguồn gốc.
    * Không tải và thực thi các file ứng dụng từ các nguồn lạ.
    * Sử dụng phần mềm có bản quyền, không dùng các công cụ bẻ khóa.
    * Quét virus trên các thiết bị nhớ lưu động (USB, thẻ nhớ,...) trước khi sử dụng.
* **Đối với hệ thống:**
    * Cập nhật các bản vá bảo mật ngay khi có thể.
    * Sử dụng firewall để chặn tất cả các cổng dịch vụ không cần thiết.
    * Phân quyền người dùng theo nguyên tắc tối thiểu hóa quyền.
    * Gia cố hệ thống, tắt các chức năng không cần thiết.
    * Cài đặt và cập nhật thường xuyên phần mềm diệt virus.
* **Đối với tổ chức:**
    * Xây dựng chính sách an toàn thông tin và đào tạo nhận thức cho người dùng.
    * Kiểm soát chặt chẽ lưu lượng mạng nội bộ để phát hiện các hành vi bất thường.