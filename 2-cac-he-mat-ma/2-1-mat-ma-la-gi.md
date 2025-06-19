# Bài 2: Các hệ mật mã
## Phần 1: Mật mã là gì?

Phần này giới thiệu các khái niệm nền tảng, mục tiêu, ứng dụng và lược sử phát triển của ngành mật mã học.

### 1.1. Khái niệm cơ bản

* **Mã hóa (Code):** Là hành động biến đổi cách thức biểu diễn thông tin. 
* **Mật mã (Cipher):**
    * **Khái niệm cũ:** Là việc mã hóa để che giấu, giữ bí mật thông tin. 
    * **Khái niệm mới:** Là các phương pháp toán học không chỉ để đảm bảo **tính bí mật** mà còn cả **tính toàn vẹn** và **tính xác thực** cho thông tin. 
* **Mật mã học (Cryptography):** Là ngành khoa học nghiên cứu các phương pháp toán học để mã hóa và bảo vệ thông tin. 
* **Thám mã (Cryptoanalysis):** Là ngành khoa học nghiên cứu các phương pháp toán học để phá vỡ các hệ mật mã. 

#### Mục tiêu của Mật mã

1.  **Tính bí mật (Confidentiality): Đảm bảo đối phương không đọc được nội dung bản tin.** 
    * **Tương tự đời thực:** Alice đặt thư vào hòm, khóa lại và gửi cho Bob. Kẻ tấn công có thể lấy được chiếc hòm nhưng không thể đọc thư nếu không có chìa khóa. 
    * **Trong mật mã:** Alice dùng một **khóa (key)** để mã hóa **bản tin gốc (plaintext)** thành **bản tin mã (ciphertext)**. Bản tin mã được gửi qua kênh không an toàn. Kẻ tấn công có thể chặn được bản tin mã nhưng không thể giải mã nếu không có khóa. 

2.  **Tính toàn vẹn (Integrity) và Xác thực (Authenticity): Đảm bảo đối phương không thể sửa đổi hoặc giả mạo bản tin.** 
    * **Cách thực hiện:** Alice tạo ra một **dấu/chữ ký xác thực** cho bản tin bằng một khóa ký. Kẻ tấn công có thể sửa bản tin nhưng không thể tạo ra dấu/chữ ký hợp lệ nếu không có khóa. Bob có thể dùng khóa xác minh để kiểm tra tính hợp lệ của bản tin. 

#### Ứng dụng của Mật mã

Mật mã là nền tảng cho rất nhiều ứng dụng quan trọng trong thế giới số: 
* Giữ bí mật cho thông tin. 
* Chữ ký số (Digital Signature). 
* Giao tiếp ẩn danh (Anonymous Communication). 
* Tiền điện tử ẩn danh (Anonymous digital cash). 
* Bầu cử điện tử (E-voting). 
* **Ví dụ điển hình:** Giao thức **HTTPS** mà bạn thấy trên trình duyệt web sử dụng mật mã để trao đổi khóa và mã hóa toàn bộ dữ liệu giữa trình duyệt (client) và máy chủ web (server), đảm bảo an toàn cho các hoạt động như kiểm tra email hay giao dịch ngân hàng. 
* **Một ví dụ khác:** Mã hóa dữ liệu trên các thiết bị lưu trữ như ổ cứng, USB. Đây có thể coi là "Alice của hôm nay gửi tin bí mật cho Alice của ngày mai". 

---

### 1.2. Lược sử Mật mã

Lịch sử mật mã là một cuộc chạy đua vũ trang không ngừng giữa người tạo mã và người phá mã.

* **Thời kỳ Cổ điển:**
    * Các kỹ thuật mật mã đã xuất hiện từ rất sớm trong các nền văn minh Ai Cập, Hy Lạp. 
    * **Mật mã Caesar:** Do Hoàng đế Julius Caesar sử dụng trong quân sự, là một hệ mật mã thay thế đơn giản bằng cách dịch chuyển ký tự đi 3 vị trí.  Ví dụ, với khóa k=3, `PARIS` sẽ được mã hóa thành `SDULV`.  Hệ mật này rất dễ bị phá vỡ. 
    * **Mật mã Vigenère:** Được phát minh vào thế kỷ 16, đây là một cải tiến lớn so với mật mã Caesar.  Nó sử dụng kỹ thuật thay thế đa bảng chữ cái, làm cho việc phân tích tần suất ký tự trở nên vô cùng khó khăn, và được coi là "le chiffrage indéchiffrable" (mật mã không thể giải mã) trong suốt 300 năm. 
* **Sự ra đời của Khoa học Mật mã (Cryptology):**
    * Vào thế kỷ 19, nhà toán học Charles Babbage và sau đó là Friedrich Kasiski đã độc lập tìm ra phương pháp phá mã Vigenère dựa trên phân tích thống kê, đánh dấu một cuộc cách mạng trong ngành. 
    * Ngành **Cryptology** ra đời, bao gồm cả **Cryptography** (tạo mã) và **Cryptoanalysis** (phá mã). 
* **Nguyên lý Kerckhoffs (1883):** Auguste Kerckhoffs đã đưa ra 6 nguyên lý thiết kế hệ mật mã, trong đó nguyên lý quan trọng nhất, vẫn còn nguyên giá trị đến ngày nay, là **Thiết kế mở (Open Design)**: "Một hệ mật mã cần phải an toàn ngay cả khi mọi thứ về nó, trừ khóa bí mật, đều được công khai". 
* **Kỷ nguyên Máy móc và Chiến tranh Thế giới:**
    * Chiến tranh Thế giới thứ hai chứng kiến sự bùng nổ của các máy mật mã cơ-điện, nổi tiếng nhất là máy **Enigma** của Đức Quốc xã. 
    * Việc phá mã Enigma thành công của đội ngũ các nhà khoa học do **Alan Turing** đứng đầu tại Bletchley Park đã góp phần quan trọng rút ngắn cuộc chiến. 
* **Mật mã Hiện đại:**
    * **Gilbert Vernam (1917):** Phát minh ra hệ mật mã Vernam, về cơ bản là toán tử XOR.  Nếu khóa được sử dụng là hoàn toàn ngẫu nhiên và chỉ dùng một lần (One-Time Pad), hệ mật này được chứng minh là **an toàn tuyệt đối**.  Tuy nhiên, việc tái sử dụng khóa sẽ làm hệ thống bị phá vỡ hoàn toàn, như đã thấy trong dự án VENONA của Hoa Kỳ. 
    * **Claude Shannon (1949):** "Cha đẻ của lý thuyết thông tin", đã đặt nền móng toán học cho mật mã học hiện đại và chính thức chứng minh tính an toàn hoàn hảo của hệ mật mã Vernam. 
    * **DES (1977):** Chuẩn mã hóa dữ liệu đầu tiên của Hoa Kỳ, sử dụng khóa 56-bit.  Do kích thước khóa ngắn, DES đã không còn an toàn trước các cuộc tấn công vét cạn và hết hạn vào năm 2005. 
    * **AES (2002):** Chuẩn mã hóa tiên tiến, được lựa chọn để thay thế DES.  AES hỗ trợ khóa 128, 192, 256 bit và hiện là chuẩn mật mã đối xứng an toàn và phổ biến nhất trên thế giới. 
    * **Mật mã khóa công khai (1976):** Whitfield Diffie và Martin Hellman đã tạo ra một cuộc cách mạng với công trình "New Directions in Cryptography", giới thiệu một phương pháp hoàn toàn mới sử dụng một cặp khóa (công khai và riêng tư) để giải quyết bài toán phân phối khóa vốn rất nan giải. 

### 1.3. Một số nguyên lý chung

* **Nguyên lý Kerckhoffs:** An toàn của một hệ mật mã phải phụ thuộc vào việc giữ bí mật của khóa, chứ không phải vào việc che giấu thuật toán. 
* **Hệ mật hoàn hảo (Perfect Secrecy):** Một hệ mật được coi là hoàn hảo nếu bản mã không tiết lộ bất kỳ thông tin gì về bản rõ.  Điều này đòi hỏi khóa phải dài ít nhất bằng bản tin và chỉ được dùng một lần. 
* **An toàn theo tính toán (Computational Security):** Do hệ mật hoàn hảo không thực tế, các hệ thống hiện đại hướng tới an toàn theo tính toán.  Tức là, hệ thống được coi là an toàn nếu kẻ tấn công không thể phá mã trong một khoảng thời gian hợp lý với các nguồn lực tính toán hiện có.