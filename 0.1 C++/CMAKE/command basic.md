
# Source

[Youtube tutorial](https://www.youtube.com/watch?v=foT2XpyHUQE&list=PLd50wmdOl6HFbPBYaApuAFjWREq4GzAK0&index=11&t=470s)


## add_library vs add_executable
[[add_library vs add_executable]]

## target_include_directories()
`target_include_directories()`: is used to specify include directories for a given target during compilation.
It means that when we link other library this target, the include directory will automatically be included

## target_link_libraries()
list library that the target depend on

```cmake
target_link_libraries(trip_service_tests
        PUBLIC TripService
        PRIVATE Catch2::Catch2
        PRIVATE GTest::gmock
        PRIVATE GTest::gtest
        )
```
- **Public Linkage (`PUBLIC`)**: Specifies that the libraries are required for the target and also exposes them to any targets that link to this target. This is useful for libraries that are essential to the functionality of the target and might also be needed by other targets.
- **Private Linkage (`PRIVATE`)**: Indicates that the libraries are required for the target but are not exposed to other targets. This is suitable for libraries that are internal to the target and not needed by consumers of the target.
- **Interface Linkage (`INTERFACE`)**: Used to specify libraries that are part of the target's interface, meaning they are required for the target but are intended to be used by other targets that link to this target. This is particularly useful for header-only libraries or libraries that provide interfaces for other targets.



## add_test()
The `add_test()` command in CMake is utilized to define tests within your project, enabling automated testing frameworks to discover and run these tests.
example:
```cpp
cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Enable testing capabilities
enable_testing()

# Define an executable
add_executable(MyExecutable main.cpp)

# Add a test for the executable
add_test(NAME MyTest COMMAND MyExecutable arg1 arg2)
```
In this example, `MyTest` is a test that executes `MyExecutable` with arguments `arg1` and `arg2`. When testing is enabled (using `enable_testing()`), CMake configures the build system to recognize this test, and tools like CTest can discover and run it.

### Running Tests

After configuring your project with CMake and adding tests, you can run the tests using CTest, which is distributed with CMake. Simply navigate to your build directory and execute:
```sh
ctest
```
This command will run all tests added with `add_test()` and report the results, including which tests passed or failed.


# Single target

[source code](https://github.com/tNhanDerivative/embedded_Class/tree/main/cmake_example/Single_Executable_Target)
file structure

```
└── Single_Executable_Target
    ├── CMakeLists.txt
    ├── include
    │   ├── dog.hpp
    │   └── operations.hpp
    ├── main.cpp
    └── src
        ├── dog.cpp
        └── operations.cpp

```