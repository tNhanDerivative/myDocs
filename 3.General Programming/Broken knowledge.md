# tìm file nằm ở đâu trong VS
tìm tên file trong mục lục folder
# Add new parameter to old function

in `EnableAutoBlocking()` in `MdproxyShared.h` when we add ignoreISO parameter we let it have default parameter =false so old function call without new parameter will auto set that variable as false
`bool ignoreISO = false`
Không cần overloading khi thêm parameter cho function.

# Đặt thêm scope trong 1 function
Lý do: 
- **Variable Lifetime Management**: Scopes are used to control the lifetime of variables. Variables declared within a scope are only valid and accessible within that scope. (e.g., automatic destruction of objects when they go out of scope).
- This can help prevent naming conflicts and ensure proper resource management .




# Đọc code
OFCfeatureManager là class con của featureManager
OFCfeatureManager sẽ register vào một cái vector trong featureManager, install feature ...

Ctrl + shift + F để tìm reference gearService.cpp gọi OFCfeatureManager

