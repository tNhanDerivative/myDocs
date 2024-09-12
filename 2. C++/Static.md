
`static`: khai báo một giá trị chiếm tồn tại cùng suốt vòng đời của chương trình, ghi nhớ giá trị thay đổi sau mỗi lần gọi
# Source

[static lý thuyết](https://techacademy.edu.vn/static-trong-c/)

# Static member
[[uart_api#Static declare]]

- Khi chúng ta khai báo 1 thành viên của 1 lớp là static, lúc đó dù cho có bao nhiêu đối tượng của lớp được tạo, thì sẽ chỉ mang 1 bản sao của thành viên static
- Tuy nhiên chúng ta không thể gán trước giá trị ban đầu của static trong class mà chúng ta phải khởi tạo giá trị cho nó ở ngoài class
- Hàm và biến khai báo static độc lập với bất kỳ đối tượng cụ thể nào của lớp. Một hàm thành viên static có thể được gọi ngay cả lúc không có đối tượng nào của lớp tồn tại và những hàm static được truy cập chỉ bởi dùng tên lớp và toán tử phân giải phạm vi ` :: `

## Static member function

Can't access non-static member variable
Can't access this->

It also has 1 copy to access static member which only has 1 unique copy

