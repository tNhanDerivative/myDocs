---
aliases:
  - Debug C++
---

# source
[Youtube tutorial](https://www.youtube.com/watch?v=Rfj40xW9q6w&t=203s)


# Inform `CMake` configuration to Vscode 

Click on `CmakeTools` extension symbol then click on `Configure all project`
`CmakeTools` will read `CMakeLists.txt` to find all the targets
![[CmakeTool_1.png]]
Bây h ta có thể debug được nhưng nếu trong code có đường dẫn đọc file các thứ đường dẫn dễ sai vì working directory của 

# config `launch.json`
Khi click vào target và chọn như dưới đây.
CMakeTool sẽ chọn target để thay vào variable `${command:cmake.launchTargetPath}` 
> Xem [[Launch.json VScode]]

![[Set_Launch_Debug_Target.png]] 