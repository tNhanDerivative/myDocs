## Link
[viblo.asia Cmake](https://viblo.asia/p/dao-dau-voi-cmake-thong-qua-vi-du-07LKXNbelV4#_vi-du-2-mot-project-voi-nhieu-directory-2)

[github: Cmake subdirectory](https://stackoverflow.com/questions/42744315/cmake-with-subdirectories)

## Thư mục ví dụ

```
FRAMGIA\luong.the.vinh@framgia0221-pc:~/Workspaces/Examples/exploringBB/extras/cmake/student$ tree
.
├── build
├── CMakeLists.txt
├── include
│   └── Student.h
└── src
    ├── mainapp.cpp
    └── Student.cpp

3 directories, 4 files
```

tất cả các file header (.h) được đặt trong thư mục `include` và tất cả các file mã nguồn (.cpp) được đặt trong thư mục `src`. 
Ngoài ra thì có cả thư mục `build` (hiện đang rỗng) được sử dụng để chứa các file binary executable và các file tạm cần thiết cho quá trình build.

```Cmake
cmake_minimum_required(VERSION 2.8.9)
project(directory_test)

include_directories(include)

file(GLOB SOURCES "src/*.cpp")

add_executable(testStudent ${SOURCES})

```

- `include_directories()`: được sử dụng để tích hợp các file header vào trong môi trường build.

- `set(SOURCE...)`: có thể được sử dụng để đặt một biến (SOURCE) chứa tất cả tên của các file source (.cpp) trong project. Tuy nhiên thì bởi mỗi một file source cần được thêm một cách thủ công nên dòng tiếp theo sẽ được dùng thay thế lệnh này và hàm set sẽ bị comment lại.

- `file()`: được sử dụng để thêm source file vào project. `GLOB` (hoặc `GLOB_RECURSE`) sẽ được sử dụng để tạo một danh sách các file thỏa mãn expression được khai báo (ví dụ: `src/*.cpp`) và thêm chúng vào biến SOURCE.

- `add_executable()`: sử dụng biến `SOURCE` thay vì việc sử dụng tham chiếu cụ thể của từng source file để build một chương trình executable là `testStudent`.