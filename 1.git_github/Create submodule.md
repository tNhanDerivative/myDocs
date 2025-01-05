
Trong bài viết này mình sẽ hướng dẫn các bạn cách sử dụng [Git Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) cho việc tái sử dụng code.


Để tạo submodule cho repository **my-project** ta dùng lệnh sau:

```bash
git submodule add https://github.com/robinhuy/my-library
```

Lệnh này sẽ tạo ra một thư mục mới là **my-library** và một file mới là **.gitmodules** trong project **my-project**. Thư mục này chính là repository **my-library** luôn, chúng ta có thể cd vào và thực hiện các lệnh fetch, pull, push, … như bình thường. Còn file **.gitmodules** sẽ chứa thông tin về submodule vừa tạo.
![](submodul_structure.png)
Ngoài ra, để cho cấu trúc thư mục được đẹp, khi tạo submodule chúng ta có thể đổi lại tên thư mục, hay cho vào trong một thư mục con cũng được. Ví dụ mình tạo submodule là folder _src/library_ thay vì _my-library_ thì dùng lệnh sau:

```bash
git submodule add https://github.com/robinhuy/my-library src/library
```

Trường hợp tạo nhầm thì các bạn có thể xóa submodule đi bằng cách chạy lệnh sau:

```bash
git rm -rf my-library
rm -rf .git/modules/my-library
```

với _my-library_ là thư mục chứa submodule (được khai báo trong file _.gitmodules_).

Chú ý mọi chỉnh sửa trong submodule muốn cập nhật vào repository chính thì cần tạo thêm commit. Như vậy khi có thay đổi từ repository **my-library** và muốn cập nhật vào trong **my-project** thì cần:

- Vào trong thư mục _src/library_ để pull code mới nhất về (hoặc có thể chỉnh sửa trong này rồi push code lên).
- Commit thay đổi ở **my-project**.
![](submodul_link.png)

Như vậy dùng Git submodules sẽ giúp chúng ta chia sẻ code giữa các project một cách dễ dàng. Tuy nhiên nhược điểm của nó là việc đồng bộ code giữa các project không diễn ra tự động mà phải chủ động update ở từng project.