https://www.youtube.com/watch?v=oHHOCdmLiII&t=445s
# Set up source file
In ESP-IDF build system, this means passing them in by the "SRCS" parameter of idf_component_register.
Note this uses a CMake variable called "srcs", this is optional you can also just list all of the source file relative paths one by one after SRCS
If you're updating, you only need one CMakeLists.txt file in each place that you had a component.mk file in the old build system
`main/CMakeList.txt`
```cmake
idf_component_register(SRCS "bt_app_av.c"
							"bt_app_core.c"
							"main.c"
INCLUDE_DIRS ".")
```

The reason for encouraging "source files not directories" is that CMake only re-runs when it is necessary, and if you add a new file to the build when using globbing then CMake will not automatically re-run 

Despite all this, it is still possible to give a list SRC_DIRS instead of SRCS - similar to the GNU Make build system. CMake will use globbing to find all the source files.
```cmake
idf_component_register(SRC_DIRS "."
						INCLUDE_DIRS ".")
```

# Set up project name

[project_folder]/CMakeList.txt
```cmake
cmake_minimum_required(VERSION 3.16)
include($ENV{IDF_PATH}/tools/cmake/project.cmake)
project(test_GPIO-esp32)
```
you can change project name here
`project([project_name])`

# Menuconfig
- Change clock speed
Go to `component config` -> `ESP System Setting` -> `CPU frequency`
![](clock_config.png)
- Change flash size (our ESP32 devkit V1 has 4MB of flash)
Go to `Serial flasher config` -> `flash size`
- build
`idf.py build`




