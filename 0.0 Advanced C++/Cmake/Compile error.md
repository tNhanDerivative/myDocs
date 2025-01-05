---
aliases:
  - compile error
  - compile debug
  - read compile error
---


# How to read

Khi build error ta tiến hành build lại thêm option `-v`để hiện chi tiết quá trình build
```sh
cmake --build build/default -v
```

đây là đoạn output error Ta đọc từ `clang++: error:` trở lên
```sh
...
[ 75%] Linking CXX executable ClientApp
cd /home/trongnhan/code_space/foolAround/try_GameEngine/build/ClientApp && /usr/bin/cmake -E cmake_link_script CMakeFiles/ClientApp.dir/link.txt --verbose=1
/usr/bin/clang++ -g CMakeFiles/ClientApp.dir/src/SandBox.cpp.o -o ClientApp   -L/home/trongnhan/code_space/foolAround/try_GameEngine/Vendors/bin  -Wl,-rpath,/home/trongnhan/code_space/foolAround/try_GameEngine/Vendors/bin ../Core/libCore.a -lSTATIC -llibspdlogd
/usr/bin/ld: cannot find -lSTATIC: No such file or directory
/usr/bin/ld: cannot find -llibspdlogd: No such file or directory
/usr/bin/ld: note to link with /home/trongnhan/code_space/foolAround/try_GameEngine/Vendors/bin/libspdlogd.a use -l:libspdlogd.a or rename it to liblibspdlogd.a
clang++: error: linker command failed with exit code 1 (use -v to see invocation)
```


Đây là gợi ý cách sửa của cmake
```
/usr/bin/ld: note to link with /home/trongnhan/code_space/foolAround/try_GameEngine/Vendors/bin/libspdlogd.a use -l:libspdlogd.a or rename it to liblibspdlogd.a
```
Nguyên nhân là do khi compile library trong linux tên library sẽ được gắn thêm prefix `lib` compile `spdlog` sẽ ra binary file tên là `libspdlog.a`
Nhưng khi CMake linking library trong linux nó sẽ tự động bỏ prefix `lib` đi