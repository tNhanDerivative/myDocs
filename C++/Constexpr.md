## it make code faster
- Remove computation from runtime
- because code don't have to check if the value is initialize or not. Constexpr guarantee to initialize at compile time.

## What i wanna do
I want a function have limited input value so I want the output of those input to be computed at compile time
## Magic
Using constexpr specifier mean it is "possible" to evaluate the value of a function or variable at compile time
The catch is it is "possible". Constexpr can run on both Compile time and Runtime. 
## How
It's basically a local compile time value.


[Lý thuyết](https://iamsorush.com/posts/cpp-constexpr/#constexpr-and-template)


## constexpr variables
[Lý thuyết](https://learn.microsoft.com/en-us/cpp/cpp/constexpr-cpp?view=msvc-170)
- A variable can be declared with **`constexpr`**, when it has a literal type and is initialized. 
- If the initialization is performed by a constructor, the constructor must be declared as **`constexpr`**
- A reference may be declared as **`constexpr`** when both these conditions are met: The referenced object is initialized by a constant expression, and any implicit conversions invoked during initialization are also constant expressions.

```cpp
constexpr float x = 42.0;
constexpr float y{108};
constexpr float z = exp(5, 3);
constexpr int i; // Error! Not initialized
int j = 0;
constexpr int k = j + 1; //Error! j not a constant expression
```

## constexpr function
When its arguments are **`constexpr`** values, a **`constexpr`** function produces a compile-time constant. When called with non-**`constexpr`** arguments, or when its value isn't required at compile time, it produces a value at run time like a regular function. (This dual behavior saves you from having to write **`constexpr`** and non-**`constexpr`** versions of the same function.)

## constexpr class member
- Should be static, not sure