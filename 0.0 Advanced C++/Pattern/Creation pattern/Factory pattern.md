
# Overview

Đọc ví dụ thực tế [[Factory pattern for window class]]

# Vấn đề phát sinh
Chúng ta cực kì khó đọc code để biết được concrete class nào được inject vào
=> Phải hoàn toàn dựa vào debug

# Problem
Bản chất Factory pattern nghĩa là chúng ta thay việc gọi object construction (sử dụng new) bằng việc gọi đến một factory method
Lý do: Class Employee muốn sử dụng object của Class `DeviceE` có interface là `iDevice`.
Class Employee sẽ sử dùng một `iDevicePointer` để sử dụng product
Chúng ta sẽ inject `DeviceE` vào `iDevicePointer`
=> vấn đề là chúng ta sẽ inject ở đâu.

# Find solution
Để Inject Device cho Class Employee muốn sử dụng: 
1. Trong execute cpp file phải create Device object  rồi truyền vào hàm Employee's construction 
2. Class contain Employee, giả sử class EngineerDepartment, phải create Device object rồi truyền vào Employee's construction 
3. hoặc Employee phải create Device object để tự sử dụng.


(Giải  thích tại sao 1 thường không khả thi )
(2 thật vô lý khi Class contain Employee phải biết (depend on) tất cả concrete device trong khi ta thậm chí ko muốn Employee phải biết (depend on) tất cả concrete device)
Khi sử dụng phương án 3 nếu làm theo logic thường thì Employee phải biết (depend) tất cả các concrete class của Device

=> tạo ra một factory để construct Device, Factory này sẽ biết (depend) tất cả các concrete class của Device, ta chỉ cần truyền cho nó một spec, một configuration; Factory sẽ trả lại concrete product vào interface pointer. Còn Employee sẽ chỉ cần biết interface ko còn depend lên các concrete class nữa.
=> separate creating object from using object để tránh việc vì creating object mà phải depend lên tất cả các concrete class

# Implement Factory pattern 

`factory.h` sẽ được include vào Employee 
`factory.h` sẽ ko chứa các concrete class nhưng `factory.cpp` chứa các concrete class
=> Employee ko depend lên các concrete class

Đọc ví dụ thực tế [[Factory pattern for window class]]

