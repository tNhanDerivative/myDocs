
# 
[Lý thuyết](https://learn.microsoft.com/en-us/cpp/cpp/constexpr-cpp?view=msvc-170)
- It may contain local variable declarations, but the variable must be initialized. It must be a literal type, and can't be **`static`** or thread-local. The locally declared variable isn't required to be **`const`**, and may mutate.
- A **`constexpr`** non-**`static`** member function isn't required to be implicitly **`const`**.