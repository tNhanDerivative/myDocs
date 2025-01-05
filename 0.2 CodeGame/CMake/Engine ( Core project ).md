---
aliases:
  - how to include
---


# Review 
1. Set minimum required C++ standard && Define some pre-processor
2. Add pre-compile header
3. Create a library target & add the source files
4. Specify include directories
5. Specify external dependencies (library) name & Specify their directories.

# Define pre-processor

```cmake
target_compile_definitions(${PROJECT_NAME} PUBLIC ON_VI_ENGINE DYNAMIC_LIB=0 DYNAMIC_BUILD=0)
```

# Add pre-compiled header
When building a game a target as a library we often want it to support both Dynamic and Static build.
PCH file defines a special label Macro that helps control how functions and classes are shared between different parts of a program.
```CMAKE
target_precompile_headers(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/VIEngine/pch.h)
```

# Declare a library target & add the source files
## Add all .cpp file 
Find all `.cpp` files in the `VIEngine` directory and make the build system dependent on those files changing

`Engine/CMakeLists.txt`
```cmake
file(GLOB_RECURSE SRC_FILES ${CMAKE_CURRENT_SOURCE_DIR}/VIEngine/*.cpp CMAKE_CONFIGURE_DEPENDS)
```

`CMAKE_CURRENT_SOURCE_DIR`: this is the directory where the currently processed `CMakeLists.txt` is located in

1. `file(GLOB_RECURSE ...)`: This is a CMake command used to recursively glob (find) files matching certain patterns.
2. `SRC_FILES`: This is the variable that will store the list of found files.
3. `${CMAKE_CURRENT_SOURCE_DIR}/VIEngine/*.cpp`: This specifies the directory and pattern to match.
4. `CMAKE_CONFIGURE_DEPENDS`: This flag tells CMake to rebuild system if any of the matched files change.

## Create Library

```cmake
add_library(${PROJECT_NAME} STATIC ${SRC_FILES})
```
`STATIC`:
`SHARED`:
# Specify target_include_directories
(Include directory of the library and the external dependencies)

3. Without specifying include directories, the compiler would look for "header.h" in the current directory and fail to find it.
4. By adding the "include" directory to the `target_include_directories`, you tell CMake to instruct the compiler to look in that directory for header files.
```cmake
target_include_directories(${PROJECT_NAME} PUBLIC
	${CMAKE_CURRENT_SOURCE_DIR}/VIEngine
)
```

Khi include 1 file header ta sẽ đặt file path relative với target_include_directory mà ta đã khai báo trong dấu <>
```cpp
#include<spdlog/sinks/stdout_color_sinks.h>
```
Còn với file nằm trong target_include_directory hoặc cùng thư mục thì chỉ cần đặt tên file trong dấu ngoặc kép
```cpp
#include"pch.h"
```

# Khai báo external Library dir and their include dir
 `${CMAKE_SOURCE_DIR}/Vendors/include` là đường dẫn chứa include directory của tất cả vendor library trong đó có spdlog. Ta include khi sử dụng như này `#include<spdlog/sinks/stdout_color_sinks.h>`
 => thêm đường dẫn này  vào [[Engine ( Core project ) # Specify target_include_directories]]
 
# spdlog




```cmake


# ${CMAKE_SOURCE_DIR}/Vendors/bin contain all precompiled *.lib and *.pdb
target_link_directories(${PROJECT_NAME} PUBLIC ${CMAKE_SOURCE_DIR}/Vendors/bin) 

target_link_libraries(${PROJECT_NAME} PUBLIC spdlog)

```