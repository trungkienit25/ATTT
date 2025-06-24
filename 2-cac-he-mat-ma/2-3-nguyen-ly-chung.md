### 1.3. Một số nguyên lý chung

Để hiểu sâu hơn về cách các hệ mật mã được thiết kế và đánh giá, chúng ta cần nắm vững một số nguyên lý nền tảng.

#### Nguyên lý Kerckhoffs

Đây là một trong những nguyên lý quan trọng và cơ bản nhất trong mật mã học hiện đại.
* **Phát biểu:** "Một hệ mật mã cần phải an toàn ngay cả khi đối phương biết mọi thông tin về hệ thống, trừ khóa bí mật". 
* **Ý nghĩa:** An toàn của một hệ mật mã không được phụ thuộc vào việc che giấu thuật toán hay thiết kế (security through obscurity).  Thay vào đó, toàn bộ sức mạnh bảo mật phải nằm ở **khóa**. 
* **Tại sao?**
    * Thuật toán có thể bị lộ qua nhiều cách: dịch ngược mã nguồn, gián điệp công nghiệp, hoặc đơn giản là nhân viên nghỉ việc.
    * Việc thay đổi thuật toán trên toàn bộ hệ thống khi bị lộ là cực kỳ tốn kém và phức tạp. Ngược lại, việc thay đổi khóa thì đơn giản hơn rất nhiều.
    * Việc công khai thuật toán cho phép cộng đồng các nhà khoa học, chuyên gia cùng phân tích, tìm ra điểm yếu và từ đó củng cố độ an toàn của thuật toán.

#### Hệ mật hoàn hảo (Perfect Secrecy)

Đây là tiêu chuẩn an toàn cao nhất, mang tính lý thuyết, do Claude Shannon định nghĩa.

* **Định nghĩa:** Một hệ mật được gọi là hoàn hảo khi và chỉ khi bản mã không tiết lộ bất kỳ thông tin nào về bản rõ.  Về mặt toán học, điều này có nghĩa là xác suất để bản rõ là `m` không thay đổi sau khi đã biết bản mã là `c`: $$Pr(M = m | C = c) = Pr(M = m)$$
* **Định lý Shannon:** Một hệ mật mã được coi là hoàn hảo khi và chỉ khi không gian khóa lớn ít nhất bằng không gian bản tin (`||K|| ≥ ||M||`). 
* **Các yêu cầu cần thiết để đạt được Hệ mật hoàn hảo:**
    1.  Kích thước của khóa phải lớn hơn hoặc bằng kích thước của bản tin. 
    2.  Khóa phải được sinh ra một cách hoàn toàn ngẫu nhiên và **chỉ được sử dụng một lần duy nhất** (One-Time Pad). 

Do các yêu cầu khắt khe này, đặc biệt là về quản lý khóa, hệ mật hoàn hảo rất tốn kém và không thực tế cho hầu hết các ứng dụng hàng ngày. 

#### An toàn theo tính toán (Computational Security)

Vì hệ mật hoàn hảo không khả thi, các hệ thống mật mã trong thực tế hướng đến một mục tiêu thực tế hơn: **an toàn theo tính toán**. 

* **Khái niệm:** Một hệ mật được coi là an toàn theo tính toán nếu kẻ tấn công, ngay cả khi có các thuật toán tấn công hiệu quả nhất, cũng không thể phá mã trong một khoảng thời gian hợp lý với các nguồn lực tính toán hiện có. 
* **Định nghĩa (t, ε)-security:** Một hệ mật được gọi là an toàn `(t, ε)` nếu bất kỳ kẻ tấn công nào thực hiện phá mã trong thời gian tối đa là `t` thì chỉ có thể đạt được xác suất thành công tối đa là `ε`. 
* **Ví dụ:** Giả sử một hệ thống được coi là an toàn nếu kẻ tấn công mất 100 năm để phá mã với xác suất thành công là $2^{-60}$.  Nếu CPU 16 GHz, khóa cần có độ dài `n` là bao nhiêu? Nếu CPU nhanh hơn 1 triệu lần, `n` cần tăng lên bao nhiêu?  (Đây là những bài toán giúp chúng ta hình dung về mối quan hệ giữa sức mạnh tính toán và độ dài khóa cần thiết).
* **Trong lý thuyết:** Một thuật toán tấn công được coi là "hiệu quả" nếu nó chạy trong thời gian đa thức `poly(n)` theo độ dài khóa `n`.  Hệ mật được coi là an toàn nếu xác suất thành công `ε` của mọi thuật toán hiệu quả là "không đáng kể" (negligible). 
    * **Thực tế:** Xác suất không đáng kể thường được coi là $ε ≤ 2^{-80}$. 

#### Lý thuyết Shannon (tiếp)

* **Độ dư thừa của ngôn ngữ:** Mọi ngôn ngữ tự nhiên (như tiếng Anh, tiếng Việt) đều có độ dư thừa.  Tức là, dựa vào các ký tự đã biết, ta có thể đoán được ký tự tiếp theo với một xác suất nào đó.  Đây chính là điểm yếu mà các nhà thám mã khai thác để phá các hệ mật mã cổ điển.

* **Khoảng cách Unicity (`u`):** Là độ dài tối thiểu của bản mã mà kẻ tấn công cần thu thập để có thể xác định duy nhất khóa đã được sử dụng.  `u` càng lớn, độ an toàn của hệ mật càng cao.  Công thức tính `u` phụ thuộc vào entropy của khóa và của ngôn ngữ bản rõ. 
* **Làm thế nào để tăng độ an toàn?** 
    * Tăng kích thước không gian khóa (tăng độ dài khóa).
    * Giảm độ dư thừa của ngôn ngữ bản rõ (ví dụ: nén dữ liệu trước khi mã hóa).

#### Thông tin tham khảo – Kích thước khóa

* **Mục tiêu:** Kích thước khóa phải đủ lớn để tấn công vét cạn (thử mọi khóa có thể) là không khả thi về mặt tính toán. 
* **Ví dụ về DES (khóa 56 bit):**
    * Năm 1998, máy tính chuyên dụng EFF DES Cracker (trị giá 250.000$) đã bẻ khóa DES trong khoảng 3 ngày. 
    * Năm 2006, hệ thống COPACOBANA (trị giá 10.000$) bẻ khóa DES trong khoảng 6.4 ngày. 
* **Khuyến nghị:** Kích thước khóa an toàn phụ thuộc vào thuật toán, sức mạnh tính toán hiện tại và dự báo trong tương lai, cũng như thời gian cần bảo mật thông tin. Các tổ chức như NIST (Viện Tiêu chuẩn và Công nghệ Quốc gia Hoa Kỳ) thường xuyên đưa ra các khuyến nghị về độ dài khóa tối thiểu cho các thuật toán khác nhau để đảm bảo an toàn cho đến năm 2030 và xa hơn. 
    * Ví dụ, để đạt mức an toàn 112 bit (an toàn đến ~2030), cần dùng 3TDEA hoặc AES-128. Để đạt mức an toàn 128 bit (an toàn sau 2030), cần dùng AES-128.