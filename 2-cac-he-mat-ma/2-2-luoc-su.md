### 1.2. Lược sử mật mã

Lịch sử mật mã là một cuộc chạy đua vũ trang không ngừng nghỉ giữa những người tạo ra các hệ thống mã hóa và những người tìm cách phá vỡ chúng.

#### Kỹ thuật mật mã cổ điển

Các kỹ thuật mật mã đã xuất hiện trong hầu hết các nền văn minh từ rất sớm.
* Người Ai Cập cổ đại đã sử dụng các chữ tượng hình không theo tiêu chuẩn để che giấu thông tin.
* Người Hy Lạp cổ đại sử dụng gậy scytale, một kỹ thuật mật mã dựa trên phép hoán vị.
* Hoàng đế Julius Caesar sử dụng kỹ thuật mật mã dựa trên phép thay thế ký tự.
* Về sau, các kỹ thuật trở nên phức tạp hơn nhưng chủ yếu vẫn là mật mã thay thế, thường được soạn thảo thành các từ điển mã (codebook). Tuy nhiên, chúng sớm bị đánh bại bởi các kỹ thuật phân tích thống kê.

#### Mật mã Vigenère

* Được phát minh bởi Giovan Battista Bellaso vào năm 1553 và được Blaise de Vigenère cải tiến quan trọng vào năm 1586.
* Đây là một bước tiến lớn, sử dụng kỹ thuật thay thế trên **nhiều bảng chữ cái** thay vì một bảng như các từ điển mã thông thường.
* Điều này tạo ra sự hỗn loạn về thống kê, vì một ký tự gốc có thể được mã hóa thành các ký tự mã khác nhau ở các vị trí khác nhau trong văn bản.
* Hệ mật này được coi là không thể phá vỡ trong khoảng 300 năm.

#### Sự dịch chuyển thành khoa học mật mã (Cryptology)

* Cho đến thế kỷ 19, mật mã vẫn chưa phát triển một cách có hệ thống.
* Năm 1846, **Charles Babbage** đã công bố phương pháp phá mã Vigenère dựa trên phân tích thống kê.
* Năm 1863, **Friedrich Kasiski** đã hoàn thiện và trình bày phương pháp này như một công trình đầy đủ.
* Sự kiện này được coi là một "cuộc cách mạng trong khoa học mật mã"  và khai sinh ra ngành **Cryptology = Cryptography (tạo mã) + Cryptanalysis (phá mã)**.

#### Nguyên lý Kerckhoffs

* Năm 1883, Auguste Kerckhoffs đã giới thiệu 6 nguyên lý thiết kế hệ mật mã.
* Trong đó, nguyên lý thứ hai đã trở thành tôn chỉ cho mật mã hiện đại và là tiêu chuẩn để đánh giá bất kỳ hệ thống an toàn thông tin nào: **Thiết kế mở (Open Design)**. Nguyên lý này phát biểu rằng: "Một hệ mật mã cần phải an toàn ngay cả khi đối phương biết mọi thông tin về hệ thống, trừ khóa bí mật".

#### Sự ra đời của máy mật mã

* Trong Thế chiến thứ hai, các **máy mật mã (rotor machine)** cơ-điện đã được chế tạo và sử dụng rộng rãi, giúp việc mã hóa/giải mã trở nên đơn giản và các phương pháp thám mã dựa trên thống kê trở nên kém hiệu quả hơn.
* Nổi tiếng nhất là máy **Enigma** của Đức. Tuy nhiên, các nhà khoa học, với sự dẫn dắt của **Alan Turing**, đã thành công trong việc phá mã Enigma, một thành tựu quan trọng góp phần vào chiến thắng của phe Đồng Minh.

#### Sự dịch chuyển sang mật mã hiện đại

* **Gilbert Vernam (1917):** Phát minh ra một phương pháp mật mã mà thiết kế mạch của nó tương ứng với toán tử **XOR**. Trên lý thuyết, hệ mật mã này là **hoàn hảo** (không thể bị phá vỡ) nếu khóa được sử dụng là hoàn toàn ngẫu nhiên và không bao giờ lặp lại (còn gọi là One-Time Pad).
* Tuy nhiên, nếu khóa bị sử dụng lại, hệ thống sẽ rất dễ bị phá vỡ. Ví dụ, dự án VENONA của Hoa Kỳ đã giải mã thành công hàng nghìn bức điện tín của tình báo Liên Xô do họ đã tái sử dụng khóa.
* **Lý thuyết Shannon (1949):** Claude E. Shannon, "Cha đẻ của lý thuyết thông tin", đã công bố công trình "Communication Theory of Secrecy Systems", đặt nền móng toán học cho mật mã học hiện đại và đưa ra khái niệm "hệ mật mã hoàn hảo" một cách chính thức.
* **DES (Data Encryption Standard - 1977):** Do IBM thiết kế, được chính phủ Hoa Kỳ chuẩn hóa để sử dụng trên máy tính điện tử. Với khóa chỉ 56 bit, DES nhanh chóng bị coi là có nguy cơ bị tấn công vét cạn (brute-force attack) và đã hết hạn vào năm 2005.
* **AES (Advanced Encryption Standard - 2002):** Do nhu cầu về một hệ mật mã an toàn hơn, chính phủ Hoa Kỳ đã khởi động một cuộc thi và lựa chọn thuật toán **Rijndael** làm chuẩn AES mới. AES hỗ trợ các kích thước khóa 128, 192, 256 bit và hiện là phương pháp mật mã đối xứng an toàn nhất cho mục đích dân sự, có khả năng chống lại tấn công vét cạn ngay cả trên máy tính lượng tử (với khóa 256 bit).
* **Sự ra đời của mật mã khóa công khai (1976):** Với sự phát triển của mạng máy tính, bài toán chia sẻ khóa bí mật trở nên cấp thiết. Năm 1976, **Whitfield Diffie và Martin Hellman** đã công bố một phương pháp trao đổi khóa hoàn toàn mới. Họ đề xuất sử dụng một cặp khóa: một **khóa công khai** (public key) được chia sẻ rộng rãi và một **khóa riêng** (private key) được giữ bí mật, tạo ra một cuộc cách mạng trong mật mã học.