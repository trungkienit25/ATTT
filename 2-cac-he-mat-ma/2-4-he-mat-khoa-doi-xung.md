## 2. Hệ mật mã khóa đối xứng (Symmetric Key Cryptography)

Hệ mật mã khóa đối xứng, còn được gọi là mật mã khóa bí mật (Secret-key cryptography), là loại mật mã mà trong đó, **cùng một khóa** được sử dụng cho cả quá trình mã hóa và giải mã.  Đây là loại mật mã đã được phát triển từ rất sớm. 

* **Nguyên tắc hoạt động:** Các thuật toán thường phối hợp các toán tử toán học đơn giản như Thay thế (Substitution), Đổi chỗ (Transposition/Permutation), và XOR. 
* **Ưu điểm:** Tốc độ thực thi các thuật toán rất nhanh và có thể được cài đặt dễ dàng bằng phần cứng. 
* **Các hệ mật hiện đại:** DES, 2DES, 3DES, AES, RC4, RC5. 

### 2.1. Sơ đồ nguyên lý

Một hệ mật mã khóa đối xứng bao gồm các thành phần sau: 
* **Bản rõ (plaintext - m):** Thông tin gốc cần được bảo vệ.
* **Bản mã (ciphertext - c):** Thông tin sau khi đã được mã hóa.
* **Khóa (key - k):** Một giá trị bí mật đã được hai bên chia sẻ an toàn từ trước.
* **Hàm sinh khóa `KeyGen()`:** Một hàm ngẫu nhiên để tạo ra khóa `k`.
* **Hàm mã hóa `E(k, m)`:** Một hàm ngẫu nhiên nhận đầu vào là khóa `k` và bản rõ `m`, đầu ra là bản mã `c`.
* **Hàm giải mã `D(k, c)`:** Một hàm xác định nhận đầu vào là khóa `k` và bản mã `c`, đầu ra là bản rõ `m`.

Hệ thống phải đảm bảo **tính đúng đắn**: `D(k, E(k, m)) = m`. 

Yêu cầu quan trọng nhất đối với khóa `k` là nó phải được sinh ra một cách ngẫu nhiên và được **chia sẻ một cách bí mật** giữa các bên. 

### 2.2. Thám mã (Các dạng tấn công)

Độ an toàn của một hệ mật mã được đánh giá qua khả năng chống lại các dạng tấn công của kẻ thám mã. Theo nguyên lý Kerckhoffs, chúng ta luôn giả định kẻ tấn công đã biết mọi thứ về thuật toán, chỉ có khóa là bí mật. 

* **Tấn công chỉ biết bản mã (Ciphertext-Only Attack - COA):** Kẻ tấn công chỉ có được các bản mã.  Đây là kịch bản yếu nhất cho kẻ tấn công.
* **Tấn công biết trước bản rõ (Known-Plaintext Attack - KPA):** Kẻ tấn công có được một số cặp (bản rõ, bản mã) tương ứng. 
* **Tấn công chọn trước bản rõ (Chosen-Plaintext Attack - CPA):** Kẻ tấn công có khả năng yêu cầu hệ thống mã hóa một số bản rõ do hắn tự chọn và nhận lại bản mã tương ứng. 
* **Tấn công chọn trước bản mật (Chosen-Ciphertext Attack - CCA):** Đây là mô hình tấn công mạnh nhất. Kẻ tấn công có quyền chọn một số bản mã (trừ bản mã mục tiêu) để yêu cầu hệ thống giải mã và nhận lại bản rõ. 

Một hệ mật mã được coi là an toàn (ví dụ, an toàn IND-CPA) nếu kẻ tấn công, ngay cả trong kịch bản CPA, cũng không thể phân biệt được bản mã của hai bản rõ khác nhau do hắn chọn, với xác suất thành công cao hơn việc đoán ngẫu nhiên một cách đáng kể. 

### 2.3. Mật mã dòng (Stream Cipher)

Mật mã dòng xử lý bản rõ theo từng đơn vị nhỏ (thường là byte) và phù hợp cho các ứng dụng truyền dữ liệu thời gian thực. 

* **Nguyên tắc:** Sử dụng một **Bộ sinh số giả ngẫu nhiên (Pseudo-Random Generator - PRG)** để mở rộng một khóa bí mật ngắn `k` thành một dòng khóa (keystream) dài. Dòng khóa này sau đó được XOR với bản rõ để tạo bản mã. 
    `c = m ⊕ PRG(k)`
* **Mật mã một lần (One-Time Pad - OTP):** Là trường hợp lý tưởng của mật mã dòng, nơi dòng khóa là hoàn toàn ngẫu nhiên và dài bằng bản rõ. Nó cung cấp **an toàn tuyệt đối (perfect secrecy)**. 
* **Lưu ý quan trọng:** Việc sử dụng lại cùng một khóa và cùng một giá trị khởi tạo (nonce/IV) để mã hóa hai bản tin khác nhau sẽ làm lộ thông tin (`c1 ⊕ c2 = m1 ⊕ m2`) và phá vỡ hoàn toàn tính an toàn của hệ thống.  Đây được gọi là lỗi "two-time-pad". 
* **Các thuật toán:** RC4 (đã cũ và không còn an toàn), Salsa20, ChaCha20.

### 2.4. Mật mã khối (Block Cipher)

Mật mã khối xử lý dữ liệu theo từng khối có kích thước cố định (ví dụ: 64 bit cho DES, 128 bit cho AES). 

* **DES (Data Encryption Standard):** Sử dụng khóa 56 bit, khối 64 bit.  Do khóa quá ngắn nên không còn an toàn trước tấn công vét cạn.  **3DES** là một cải tiến bằng cách thực hiện DES ba lần với 2 hoặc 3 khóa khác nhau để tăng độ dài khóa hiệu dụng. 
* **AES (Advanced Encryption Standard):** Là chuẩn thay thế cho DES.  AES hỗ trợ khóa 128, 192, 256 bit và khối 128 bit, được coi là rất an toàn và hiệu quả. 

#### Chế độ hoạt động của mã khối

Vì bản thân mã khối là có tính xác định (cùng một đầu vào luôn cho ra cùng một đầu ra), nó cần được sử dụng trong các "chế độ hoạt động" để có thể mã hóa các bản tin dài hơn một khối và đảm bảo tính an toàn.

* **ECB (Electronic Codebook):** Mã hóa mỗi khối một cách độc lập.
    * **Nhược điểm:** **Rất không an toàn**. Nếu hai khối bản rõ giống nhau, hai khối bản mã cũng sẽ giống hệt nhau, làm lộ mẫu dữ liệu.  **Không bao giờ được sử dụng chế độ này.**
* **CBC (Cipher Block Chaining):** Trước khi mã hóa, mỗi khối bản rõ được XOR với khối bản mã ngay trước đó. 
    * Khối đầu tiên được XOR với một giá trị ngẫu nhiên gọi là **Vector Khởi tạo (Initialization Vector - IV)**.
    * Việc sử dụng IV ngẫu nhiên giúp cho hai lần mã hóa cùng một bản tin sẽ cho ra hai bản mã khác nhau, qua đó đạt được an toàn CPA. 
* **CTR (Counter Mode):** Biến một mã khối thành một mã dòng. 
    * Nó mã hóa các giá trị liên tiếp của một bộ đếm (bắt đầu bằng một giá trị ngẫu nhiên `nonce`) để tạo ra một dòng khóa, sau đó XOR với bản rõ. 
    * **Ưu điểm:** Có thể thực hiện mã hóa và giải mã song song, không yêu cầu đệm (padding) bản tin. 

* **Phần đệm (Padding):** Khi khối cuối của bản tin không đủ độ dài, cần phải thêm các byte đệm.  Chuẩn PKCS#7 quy định rằng nếu cần đệm `n` byte thì giá trị của mỗi byte đệm sẽ là `n`. 

#### Tấn công vào mã khối

* **Tấn công vét cạn (Exhaustive Search):** Thử mọi khóa có thể. Không khả thi với AES nhưng khả thi với DES. 
* **Tấn công kênh bên (Side-channel attack):** Khai thác thông tin rò rỉ từ quá trình triển khai vật lý của thuật toán, như thời gian thực thi, lượng điện năng tiêu thụ, hay bức xạ điện từ.