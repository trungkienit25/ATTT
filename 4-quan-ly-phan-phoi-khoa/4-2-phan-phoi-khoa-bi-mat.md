### 2. Các giao thức phân phối khóa bí mật (Symmetric Key Distribution Protocols)

Một trong những thách thức lớn nhất của mật mã khóa đối xứng là làm thế nào để hai bên có thể chia sẻ một khóa bí mật chung một cách an toàn.  Phần này sẽ xem xét các giao thức được thiết kế để giải quyết vấn đề này.

#### 2.1. Giao thức phân phối khóa không tập trung (Decentralized)

Trong mô hình này, hai bên tự trao đổi khóa với nhau mà không cần một máy chủ trung gian.

* **Kịch bản:** Giả sử Alice (A) và Bob (B) đã có sẵn một **khóa chính (master key)** là `kM`.  Họ muốn sử dụng `kM` để trao đổi một **khóa phiên (session key)** là `kS` cho một lần làm việc. 
* **Giao thức 1.1 (Cơ bản):**
    1.  A → B: `IDA` (Định danh của A) 
    2.  B → A: `E(kM, IDB || kS)` (B gửi lại `kS`, được mã hóa bằng `kM`) 
* **Phân tích:** Giao thức này không an toàn vì nó dễ bị **tấn công phát lại (Replay Attack)**.  Kẻ tấn công có thể ghi lại một thông điệp số (2) từ một phiên cũ (chứa khóa `ks_old`) và phát lại nó ở phiên hiện tại để lừa A sử dụng lại khóa cũ mà kẻ tấn công đã biết. 
* **Giao thức 1.2 (Cải tiến với Nonce):**
    Để chống tấn công phát lại, giao thức được bổ sung các **giá trị dùng một lần (nonce)**. 
    1.  A → B: `IDA || N1` (`N1` là nonce do A tạo) 
    2.  B → A: `E(kM, IDB || kS || N1 || N2)` (B gửi lại `kS`, `N1` và nonce `N2` của mình) 
    3.  A → B: `E(kS, N2)` (A giải mã, kiểm tra `N1` có đúng là của mình không, sau đó gửi lại `N2` đã được mã hóa bằng `kS` để chứng minh mình đã có khóa) 
    4.  B kiểm tra `N2` để hoàn tất xác thực. 

* **Hạn chế:** Mô hình không tập trung yêu cầu mỗi cặp người dùng phải có một khóa chính riêng, dẫn đến số lượng khóa cần quản lý tăng theo cấp số nhân. 

---

#### 2.2. Sơ đồ trao đổi khóa Diffie-Hellman

Đây là một phương pháp kinh điển cho phép hai bên tạo ra một khóa bí mật chung qua một kênh không an toàn mà không cần chia sẻ khóa trước.

* **Thiết lập:** Alice và Bob cùng thỏa thuận công khai về một số nguyên tố `p` và một số sinh `g`. 
* **Quy trình:**
    1.  Alice chọn một số bí mật `XA` và tính giá trị công khai `YA = g^XA mod p`. 
    2.  Bob chọn một số bí mật `XB` và tính giá trị công khai `YB = g^XB mod p`. 
    3.  Họ trao đổi các giá trị công khai `YA` và `YB` cho nhau. 
    4.  Alice tính khóa chung: `kS = YB^XA mod p = (g^XB)^XA mod p = g^(XA*XB) mod p`. 
    5.  Bob tính khóa chung: `kS = YA^XB mod p = (g^XA)^XB mod p = g^(XA*XB) mod p`. 
    Kết quả là cả hai đều có cùng một khóa bí mật `kS` mà kẻ nghe lén không thể tính được.
* **Tấn công:** Giao thức Diffie-Hellman gốc dễ bị **tấn công Người đứng giữa (Man-in-the-Middle)**.  Kẻ tấn công (C) có thể xen vào giữa, trao đổi khóa riêng với Alice và Bob, sau đó đọc và chuyển tiếp toàn bộ thông tin giữa hai người.  Để chống lại tấn công này, cần phải có cơ chế xác thực các giá trị công khai `YA` và `YB`.

---

#### 2.3. Giao thức phân phối khóa tập trung (Centralized)

Mô hình này sử dụng một bên thứ ba tin cậy gọi là **Trung tâm Phân phối Khóa (Key Distribution Centre - KDC)**. 

* **Thiết lập:** Mỗi người dùng (ví dụ: A, B) chia sẻ một khóa bí mật riêng với KDC (lần lượt là `kA`, `kB`). 
* **Vai trò của KDC:** Khi A muốn liên lạc với B, KDC sẽ tạo ra một khóa phiên `kS` và phân phối nó một cách an toàn cho cả A và B. 
* **Giao thức Needham-Schroeder (Giao thức 2.2):** Một giao thức kinh điển sử dụng nonce để chống tấn công phát lại.
    1.  A → KDC: `IDA || IDB || N1` (A yêu cầu một khóa phiên để nói chuyện với B) 
    2.  KDC → A: `E(kA, kS || IDB || N1 || E(kB, IDA || kS))`
        * KDC trả lời A, mã hóa bằng `kA`. Gói tin chứa:
            * Khóa phiên mới `kS`.
            * Định danh của B (`IDB`) và nonce `N1` để A xác thực.
            * Một "vé" (ticket) được mã hóa bằng `kB`, chứa `kS` và `IDA`, để A gửi cho B. 
    3.  A → B: `E(kB, IDA || kS)` (A gửi "vé" cho B) 
    4.  B → A: `E(kS, N2)` (B giải mã vé, có được `kS`, sau đó gửi một nonce `N2` để thách thức A) 
    5.  A → B: `E(kS, f(N2))` (A biến đổi `N2` và gửi lại để chứng minh mình cũng có `kS`) 
* **Các cải tiến:** Các giao thức sau này như của Denning (Giao thức 2.3) đã bổ sung **nhãn thời gian (timestamp)** để chống lại việc phát lại các "vé" cũ.  Giao thức Kehne (Giao thức 2.4) cải tiến hơn nữa để không phụ thuộc vào việc đồng bộ đồng hồ giữa các bên.