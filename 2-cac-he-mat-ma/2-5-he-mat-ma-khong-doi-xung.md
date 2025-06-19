## 3. Hệ mật mã khóa bất đối xứng (Asymmetric Key Cryptography)

Hệ mật mã khóa bất đối xứng, hay còn gọi là **mật mã khóa công khai (Public Key Cryptography)**, ra đời như một cuộc cách mạng nhằm giải quyết những hạn chế cố hữu của mật mã khóa đối xứng.

#### Những hạn chế của mật mã khóa đối xứng

1.  **Vấn đề phân phối khóa:** Cần một kênh an toàn để chia sẻ khóa bí mật giữa các bên. Đây là bài toán con gà-quả trứng: làm sao để chia sẻ khóa một cách an toàn cho lần đầu tiên?.
2.  **Yêu cầu online:** Quá trình trao đổi khóa và dữ liệu thường đòi hỏi cả hai bên phải cùng trực tuyến.
3.  **Quản lý khóa phức tạp:** Trong một mạng có `n` người dùng, để mỗi cặp người dùng có một khóa riêng thì cần quản lý tới `n(n-1)/2` khóa.
4.  **Không thể xác thực thông tin quảng bá:** Khó có thể xác thực nguồn gốc của một thông điệp được gửi cho nhiều người.

### 3.1. Sơ đồ nguyên lý

Hệ mật mã khóa bất đối xứng sử dụng một **cặp khóa** cho mỗi người dùng:
* **Khóa công khai (Public Key - kU):** Được công bố rộng rãi cho tất cả mọi người. Dùng để mã hóa thông điệp hoặc xác thực chữ ký.
* **Khóa cá nhân (Private Key - kR):** Được người sở hữu giữ tuyệt đối bí mật. Dùng để giải mã thông điệp hoặc tạo chữ ký.

**Nguyên tắc cơ bản:** Thông tin được mã hóa bằng khóa này thì chỉ có thể được giải mã bằng khóa còn lại trong cặp.

* **Tính đúng đắn:** `D(kR, E(kU, m)) = m`.
* **Cơ sở an toàn:** Độ an toàn của các hệ mật này dựa trên các bài toán toán học được cho là "khó", tức là không có lời giải trong thời gian đa thức, ví dụ như bài toán phân tích một số nguyên lớn thành thừa số nguyên tố.
* **Các thuật toán phổ biến:** RSA, El-Gamal, Mật mã đường cong Elliptic (ECC).

### 3.2. Hệ mật mã RSA

Được đặt tên theo ba tác giả Rivest, Shamir, và Adleman, RSA là hệ mật mã khóa công khai đầu tiên và phổ biến nhất.

#### Sinh khóa
1.  Chọn 2 số nguyên tố lớn ngẫu nhiên `p` và `q`.
2.  Tính `n = p × q` và `Φ(n) = (p-1) × (q-1)`.
3.  Chọn một số `e` sao cho `1 < e < Φ(n)` và `UCLN(Φ(n), e) = 1`.
4.  Tính số `d` sao cho `(e × d) mod Φ(n) = 1`.
5.  **Khóa công khai** là `kU = (e, n)`.
6.  **Khóa cá nhân** là `kR = (d, n)`.

#### Mã hóa và Giải mã
* **Mã hóa:** `c = m^e mod n`.
* **Giải mã:** `m = c^d mod n`.

#### Các vấn đề của RSA
Phiên bản RSA "sách giáo khoa" (textbook RSA) như trên là có **tính xác định**, tức là cùng một bản rõ `m` sẽ luôn được mã hóa thành cùng một bản mã `c`. Điều này làm nó không an toàn, đặc biệt khi không gian bản rõ là nhỏ (kẻ tấn công có thể mã hóa thử tất cả các bản rõ có thể để tìm ra bản gốc).

Để khắc phục, trong thực tế người ta sử dụng các **lược đồ đệm (padding scheme)** như **RSA-OAEP** (theo chuẩn PKCS#1). Lược đồ này thêm các giá trị ngẫu nhiên vào bản rõ trước khi mã hóa, làm cho quá trình mã hóa trở nên ngẫu nhiên và an toàn trước các tấn công mạnh như CCA.

### 3.3. Hệ mật mã đường cong Elliptic (ECC)

ECC là một phương pháp mật mã khóa công khai hiện đại, được xem là sự thay thế hiệu quả cho RSA.

* **Cơ sở toán học:** Dựa trên các phép toán trên một đường cong Elliptic có dạng `y^2 = x^3 + ax + b` trên một trường hữu hạn (trường Galois).
* **Sinh khóa:**
    1.  Chọn một đường cong Elliptic và một điểm gốc `P` trên đó.
    2.  Người dùng chọn một số nguyên ngẫu nhiên `k` làm **khóa cá nhân**.
    3.  Tính toán `Q = kP` (phép nhân vô hướng, tức là cộng điểm `P` với chính nó `k` lần) để ra **khóa công khai**.
* **Độ an toàn:** Dựa trên độ khó của **Bài toán Logarithm rời rạc trên đường cong Elliptic (ECDLP)**: cho trước `P` và `Q`, việc tìm ra `k` là cực kỳ khó khăn về mặt tính toán.
* **Ưu điểm:** ECC cung cấp mức độ an toàn tương đương RSA nhưng với kích thước khóa nhỏ hơn rất nhiều. Ví dụ, một khóa ECC 256-bit có độ an toàn tương đương với một khóa RSA 3072-bit. Điều này giúp tiết kiệm băng thông, bộ nhớ và năng lượng tính toán, đặc biệt quan trọng cho các thiết bị di động và IoT.

### 3.4. Kết hợp mật mã KCK và mật mã KĐX (Hệ mật mã lai)

Trong thực tế, người ta không dùng mật mã khóa công khai để mã hóa lượng lớn dữ liệu vì nó chậm hơn đáng kể so với mật mã khóa đối xứng. Thay vào đó, một **sơ đồ "lai" (hybrid)** được sử dụng để tận dụng ưu điểm của cả hai loại:

1.  **Phía gửi:**
    * Tạo một khóa đối xứng ngẫu nhiên, chỉ dùng một lần (gọi là **khóa phiên - session key `kS`**).
    * Dùng khóa đối xứng `kS` và một thuật toán nhanh như AES để mã hóa bản tin `m`.
    * Dùng **khóa công khai `kUB`** của người nhận để mã hóa khóa phiên `kS`.
    * Gửi cả (bản tin đã được mã hóa bằng AES) và (khóa phiên đã được mã hóa bằng RSA/ECC) cho người nhận.

2.  **Phía nhận:**
    * Dùng **khóa cá nhân `kRB`** của mình để giải mã, lấy ra khóa phiên `kS`.
    * Dùng khóa phiên `kS` để giải mã bản tin và đọc nội dung.

Sơ đồ này vừa giải quyết được bài toán phân phối khóa an toàn của mật mã đối xứng, vừa đảm bảo hiệu năng cao khi mã hóa lượng lớn dữ liệu.