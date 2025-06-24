### 2. Virus máy tính (Computer Virus)

#### 2.1. Cách thức hoạt động của virus

* Một virus máy tính thông thường bao gồm ba đoạn mã chính:
    * **Đoạn mã lây nhiễm (Infection code):** Cho phép virus tự sao chép bản thân và lây nhiễm từ chương trình này sang chương trình khác.
    * **Đoạn mã kích hoạt (Trigger code):** Là các sự kiện hoặc điều kiện xác định khi nào hoạt động phá hoại chính sẽ được kích hoạt.
    * **Đoạn mã hoạt động (Payload code):** Là phần thực hiện các hành động phá hoại của virus.
* Virus được mô tả với hai đặc trưng chính: cách thức lây nhiễm và các hành vi phá hoại.

#### 2.2. Cách thức lây lan và cơ chế tiêm nhiễm

* **Lây lan:**
    * Virus cần hành động kích hoạt của người dùng để lây nhiễm, ví dụ như khi người dùng mở một tệp tin đính kèm email, chạy một chương trình ứng dụng đã bị nhiễm, hoặc khởi động từ một ổ cứng có phân vùng khởi động (boot sector) đã bị nhiễm.
    * Sau khi được kích hoạt, virus sẽ tìm cách lây nhiễm sang các hệ thống khác, ví dụ như tiêm nhiễm vào các tệp tin khác, lây qua các thiết bị nhớ di động, hoặc tự gửi email có đính kèm mã độc.
* **Cơ chế tiêm nhiễm:**
    * Nguyên tắc cơ bản là virus sẽ thay thế lệnh đầu tiên của tệp tin bị nhiễm bằng một lệnh `JUMP` để nhảy tới đoạn mã thực thi của virus.
    * Sau khi thực thi xong, cuối đoạn mã của virus sẽ có một lệnh `JUMP` khác để nhảy tới lệnh đầu tiên ban đầu của chương trình, giúp chương trình có vẻ vẫn hoạt động bình thường.

#### 2.3. Phát hiện virus và Các kỹ thuật lẩn tránh

* **Phát hiện dựa trên đặc trưng (Signature-based Detection):**
    * Đây là phương pháp phổ biến nhất. Các phần mềm diệt virus sẽ thu thập các mẫu virus và xây dựng một cơ sở dữ liệu chứa các "đặc trưng" (signature) của chúng, thường là các đoạn mã lây nhiễm.
    * Để phát hiện, phần mềm sẽ quét các tệp tin và so sánh mã của chúng với cơ sở dữ liệu đặc trưng đã có.
* **Các kỹ thuật lẩn tránh của virus:** Để chống lại việc phát hiện dựa trên đặc trưng, virus sử dụng các kỹ thuật để thay đổi mã nguồn của chúng.
    * **Virus đa hình (Polymorphic Virus):** Virus tự thay đổi mã nguồn một cách ngẫu nhiên mỗi khi lây nhiễm. Nó thường có một đoạn mã giải mã (decryptor) và phần thân được mã hóa. Khi lây nhiễm, nó sẽ dùng một khóa mới để mã hóa lại phần thân, khiến cho "đặc trưng" của nó liên tục thay đổi.
    * **Virus siêu đa hình (Metamorphic Virus):** Đây là loại virus còn tinh vi hơn. Nó không mã hóa mà sử dụng các kỹ thuật biến đổi mã để tự viết lại hoàn toàn mã nguồn của mình về mặt ngữ nghĩa mỗi khi lây nhiễm. Nó có thể thêm vào các đoạn mã rác, thay đổi thứ tự các câu lệnh, hoán đổi thanh ghi, hoặc thay thế các thuật toán bằng những thuật toán tương đương. Điều này khiến việc phát hiện dựa trên đặc trưng gần như không thể.

#### 2.4. Phát hiện các virus tinh vi

* Để phát hiện các virus siêu đa hình, cần sử dụng phương pháp **phát hiện dựa trên hành vi (Behavior-based detection)**.
    * **Phân tích động (Dynamic Analysis):** Thực thi mã độc trong một môi trường an toàn, được cô lập (gọi là **sandbox**) và quan sát các hành động của nó (ví dụ: các file nó tạo ra, các kết nối mạng nó thực hiện, các giá trị registry nó thay đổi).
    * **Phân tích tĩnh (Static Analysis):** Sử dụng các kỹ thuật dịch ngược (reverse engineering) để phân tích mã thực thi mà không cần chạy nó, nhằm hiểu rõ tất cả các cơ chế hoạt động.

#### 2.5. Rootkit/Stealth Virus

* **Rootkit** là loại malware có khả năng ẩn mình một cách tinh vi trước các phần mềm phát hiện.
* **Cơ chế:** Chúng sử dụng kỹ thuật "hook" để chặn các sự kiện của hệ điều hành (như lời gọi hệ thống, hàm thư viện) và can thiệp vào quá trình xử lý để che giấu sự hiện diện của chúng.
* **Phân loại:**
    * **User-level rootkit:** Hook vào các hàm thư viện, dễ bị phát hiện hơn.
    * **Kernel-level rootkit:** Hook vào các thành phần ở mức nhân hệ điều hành, driver, firmware. Loại này rất khó bị phát hiện.
    * **Virtualization-based rootkit:** Ẩn mình trong một lớp ảo hóa, gần như không thể bị phát hiện bởi hệ điều hành bên trên.
* **Phát hiện:** Cần các công cụ chuyên dụng để phát hiện các hành vi hook hoặc so sánh trạng thái của hệ thống với một hệ thống tham chiếu "sạch".