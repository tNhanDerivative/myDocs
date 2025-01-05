
`WinMain` là conventional entry-point function cho Windows-based graphical app. Nó sẽ được hệ thống tự động call khi một program start

Khi ta muốn start một program mà ko bật console lên ta cần
Thông báo cho C++ runtime library sử dụng WinMain làm entry-point bằng cách thêm dòng này vào `CMakeLists.txt`
```cmake
set_target_properties(winmain PROPERTIES LINK_FLAGS "/SUBSYSTEM:WINDOWS")
```

[Kiểm tra ví dụ này](https://github.com/meemknight/windowsAPIforGamedevelopers/blob/master/day1/winmain.cpp)