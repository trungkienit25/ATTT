### 4. Lập trình an toàn (Secure Programming)

Hiểu về các lỗ hổng là bước đầu tiên. Bước tiếp theo và quan trọng hơn là làm thế nào để viết mã nguồn nhằm ngăn chặn các lỗ hổng đó ngay từ đầu.

#### 4.1. Ngôn ngữ an toàn và không an toàn với bộ nhớ

* **Ngôn ngữ an toàn với bộ nhớ (Memory-safe):** Là các ngôn ngữ được thiết kế để tự động kiểm tra biên truy cập và ngăn cản các truy cập bộ nhớ không hợp lệ. Điều này giúp ngăn chặn được toàn bộ các lỗ hổng truy cập bộ nhớ như tràn bộ đệm. 
    * **Ví dụ:** Java, C#, Python, Go, Rust.
* **Ngôn ngữ không an toàn với bộ nhớ (Memory-unsafe):** Là các ngôn ngữ cho phép lập trình viên thao tác trực tiếp với con trỏ và địa chỉ bộ nhớ mà không có sự kiểm tra tự động, điển hình là C/C++. 
* **Tại sao C/C++ vẫn được sử dụng rộng rãi?**
    * Nguyên nhân thường được đề cập là vì hiệu năng của chương trình viết bằng C/C++ tốt hơn.  Tuy nhiên, ngày nay nhiều ngôn ngữ an toàn như Go và Rust đã có hiệu năng tương đương C trong phần lớn các tác vụ. 
    * Nguyên nhân thực tế hơn là phần lớn các hệ thống lớn, lâu đời (hệ điều hành, phần mềm nhúng,...) đã được viết bằng C từ ban đầu. 

#### 4.2. An toàn truy cập bộ nhớ

Lập trình an toàn đòi hỏi phải đảm bảo hai loại an toàn khi truy cập bộ nhớ.

* **An toàn không gian (Spatial Safety):** Đảm bảo một thao tác chỉ truy cập vào đúng vùng nhớ đã được định danh cho nó.  Một con trỏ `p` truy cập vào một vùng nhớ có địa chỉ bắt đầu là `b` và địa chỉ kết thúc là `e` chỉ được coi là an toàn khi `b ≤ p ≤ e – s`, với `s` là kích thước của vùng nhớ mà con trỏ `p` truy cập tới. 
    * Ví dụ, truy cập `str[10]` trong một mảng `char str[10]` là vi phạm an toàn không gian vì chỉ số hợp lệ của mảng là từ 0 đến 9. 
* **An toàn thời gian (Temporal Safety):** Đảm bảo một thao tác chỉ truy cập vào vùng nhớ đã được khởi tạo. 
    * **Các vi phạm phổ biến:**
        * Sử dụng một biến chưa được gán giá trị ban đầu. 
        * Sử dụng một con trỏ chưa được cấp phát bộ nhớ (con trỏ hoang). 
        * Sử dụng một con trỏ sau khi vùng nhớ mà nó trỏ tới đã được giải phóng (lỗi "use-after-free"). Để tránh lỗi này, một thói quen tốt là gán con trỏ về `NULL` ngay sau khi giải phóng bộ nhớ. 

#### 4.3. Các nguyên tắc lập trình an toàn

1.  **Kiểm tra mọi dữ liệu đầu vào:** Không bao giờ tin cậy vào dữ liệu đến từ bên ngoài chương trình của bạn, bao gồm: giá trị do người dùng nhập, nội dung file được mở, các gói tin nhận từ mạng, dữ liệu từ thư viện của bên thứ ba, v.v. 
2.  **Sử dụng các hàm an toàn:**
    * Luôn ưu tiên sử dụng các hàm xử lý xâu có kiểm tra kích thước bộ đệm như `strlcpy()`, `strlcat()`, `fgets()` thay cho các hàm không an toàn như `strcpy()`, `strcat()`, `gets()`. 
    * Luôn đảm bảo các xâu ký tự được kết thúc bằng ký tự `\0`. 
    * Nếu có thể, hãy sử dụng các thư viện an toàn hơn như `std::string` trong C++ thay vì xử lý mảng `char` thủ công. 
3.  **Sử dụng con trỏ một cách cẩn trọng:**
    * Phải hiểu rõ về các phép toán trên con trỏ. 
    * Sau khi gọi `free(p)`, hãy gán `p = NULL;` ngay lập tức để tránh lỗi con trỏ lơ lửng (dangling pointer) và use-after-free. 
4.  **Hạn chế quyền truy cập:** Che giấu các thành phần bên trong của một cấu trúc hoặc đối tượng (nguyên lý đóng gói) để hạn chế khả năng kẻ khác có thể can thiệp vào. 
5.  **Luôn cập nhật và sử dụng các chuẩn mới:** Nên sử dụng các chuẩn ngôn ngữ mới như C/C++11 trở lên vì chúng cung cấp nhiều tính năng an toàn hơn.