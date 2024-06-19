
# Source

[github question](https://stackoverflow.com/questions/3967177/when-to-use-const-and-const-reference-in-function-args#:~:text=The%20general%20rule%20is%2C%20use,not%20immutable%20in%20C%2B%2B.)

# Const pass by value is a pure implementation detail

```cpp
void f(T);
void f(T const);
```

These declarations are actually the _exact same function!_ When passing by value, const is purely an implementation detail. [Try it out:](http://codepad.org/ReB7ePO8)

```cpp
void f(int);
void f(int const) {/*implements above function, not an overload*/}

typedef void C(int const);
typedef void NC(int);
NC *nc = &f;  // nc is a function pointer
C *c = nc;  // C and NC are identical types
```

# Const pass by 
Using `const` on function parameters in C++ serves several purposes:

1. **Prevents Modification**: When you declare a function parameter as `const`, it means that the function promises not to modify the argument. 
   This is particularly useful when passing large objects to functions, as it avoids copying the entire object just to ensure immutability within the function scope. Instead, the compiler can optimize the call by passing a reference or pointer to the original object, which is much more efficient.

2. **Type Safety**:  acts as a form of documentation, signaling to other programmers that the function does not alter the state of its arguments.

3. **Compiler Optimizations**: By marking arguments as `const`, you allow the compiler to make certain optimizations. For instance, if a function takes a `const` reference to an object, the compiler knows that the function will not modify the object, potentially allowing it to avoid creating temporary copies of the object.

4. **Overloading Functions**: Using `const` in function signatures is crucial for function overloading based on const-correctness. Without `const`, the compiler would treat calls to overloaded functions as ambiguous if both versions could accept the same argument type. Marking one version as taking a `const` argument resolves this ambiguity.

Here's an example demonstrating the use of `const` in function parameters: