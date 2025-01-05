
Based on the search results and the question, here's an explanation of when we typically need to use `extern "C"` 

## 1. Khi gọi C functions trong C++ code:

We need to use `extern "C"` to declare C functions that we want to call from C++ code. This prevents name mangling and ensures proper linkage between C and C++ code [1][2].

Example:

```cpp
// In C++ code
extern "C" void foo(int x);
```

## 2. Khi gọi C++ functions trong C code:

We use `extern "C"` to declare C++ functions that we want to call from C code [2]. This ensures that the function names are not mangled.

Example:

```cpp
// In C++ header file
extern "C" {
    void bar(double d);
}
```

## 3. Include C headers trong C++ code:

We often need to wrap `#include` statements for non-standard C headers in C++ using `extern "C"` [2]. This prevents name mangling for functions declared in those headers.

Example:

```cpp
// In C++ code
extern "C" {
    #include "my-C-header.h"
}
```

## 4. Tạo wrapper functions for C++ functions:

If we want C code to call overloaded C++ functions, we need to create wrapper functions declared with `extern "C"` [2].

Example:

```cpp
// In C++ header file
extern "C" int foo_i(int i) { return foo(i); }
extern "C" double foo_d(double d) { return foo(d); }
```

## 5. When interfacing with Windows API:

While not always necessary, using `extern "C"` can be beneficial when creating functions that will be part of the Windows API or when interfacing with other libraries that expect C-style function names [1][2].

In summary, `extern "C"` is primarily used to prevent name mangling and ensure proper linkage between C and C++ code, especially when interfacing with external libraries or creating functions that need to be callable from both C and C++. In Windows programming, it's often used when dealing with the Windows API or third-party C libraries.

Citations:
[1] https://stackoverflow.com/questions/36785157/how-does-extern-c-allow-c-code-in-a-c-file
[2] https://isocpp.org/wiki/faq/mixing-c-and-cpp
[3] https://learn.microsoft.com/en-us/cpp/cpp/extern-cpp?view=msvc-170
[4] https://cplusplus.com/forum/beginner/51272/
[5] https://www.quora.com/How-do-I-use-a-C-library-in-a-C-program-properly
[6] https://learn.microsoft.com/en-us/cpp/build/exporting-c-functions-for-use-in-c-or-cpp-language-executables?view=msvc-170
[7] https://forum.xojo.com/t/c-c-external-dynamic-library-linkage-example-project/40143
[8] https://www.quora.com/How-can-I-use-my-C++-libraries-in-a-C-program
[9] https://cython.readthedocs.io/en/latest/src/userguide/external_C_code.html
[10] https://help.hcl-software.com/dom_designer/9.0.1/appdev/LSAZ_WORKING_WITH_EXTERNAL_C_LANGUAGE_FUNCTIONS.html