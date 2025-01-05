# Install GLFW lib

install Third Party lib as git `submodule`
```sh
git submodule add git@github.com:glfw/glfw.git
```

Inside `Core/Vendors/glfw/CMakeLists.txt`. Set this option ON if want to build as dynamic Lib
```CMake
option(BUILD_SHARED_LIBS "Build shared libraries" OFF)
```

`add_subdirectory` glfw to be compiled in the Solution
```cmake
add_subdirectory(Vendors/glfw)
```


# Thiết kế class Window với Factory patern

[[Factory pattern for window class]]
