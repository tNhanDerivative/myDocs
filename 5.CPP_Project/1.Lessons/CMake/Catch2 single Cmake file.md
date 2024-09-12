
Source: [video tutorial](https://www.youtube.com/watch?v=6Uq0h_wX8_o&t=98s)
[github repo](https://github.com/erichschroeter/cmake-catch2-example)


```cmake
#
# Create a library target for the Catch header-only test framework.
#
add_library(Catch INTERFACE)
target_include_directories(Catch
	INTERFACE
		test/
)
```
The term "header-only test framework" refers to a C++ library or framework that can be used without requiring separate compilation of source files. In this case, Catch2 is a popular header-only testing framework for C++
INTERFACE: keyword  meaning they are required for the target but are intended to be used by other targets that link to this target
`INTERFACE` is used here for two important reasons:
1. `add_library(Catch INTERFACE)` It indicates that Catch is a header-only library. There's no source code to compile, only headers to include.
2. It specifies that the properties set for this target (like include directories) should be used by targets that link to Catch, but not by Catch itself.
By using INTERFACE, we're telling CMake that this library doesn't produce any artifacts itself, but it provides interface properties (like include directories) that should be propagated to any targets that use it.
This setup allows other parts of the project to easily include and use the Catch2 framework by simply linking against the Catch target, without needing to worry about compilation or additional setup


## unit_test target

```cmake
add_executable(unit_tests
	test/main.cpp
	test/test_Hello.cpp
)
```
For the unit_tests we create a `Executable target` this target need its own main function, which provided by `catch` library. We can enable in the `.cpp` file like this

`main.cpp`
```cpp
#define CATCH_CONFIG_MAIN
#include "catch.hpp"
```

this target depend on `Catch` and `hello` lib
```cmake
target_link_libraries(unit_tests
	Catch
	hello
)
```

Load `ParseAndAddCatchTests` module provide by Catch then call `ParseAndAddCatchTests(unit_tests_target)` function of that module.
This function will create test entry for each of the test case detected in our unit test
```cmake
# Load and use the .cmake file provided by Catch so all the test cases
# are made available to CTest.
include(ParseAndAddCatchTests)
ParseAndAddCatchTests(unit_tests)
```

## Install
package unit_tests target in the bin directory
```cmake
install(
	TARGETS unit_tests
	RUNTIME DESTINATION bin
)
```
