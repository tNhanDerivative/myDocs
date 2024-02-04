
# Source
[ github question](https://stackoverflow.com/questions/57483/what-are-the-differences-between-a-pointer-variable-and-a-reference-variable)

A pointer can be assigned `nullptr`, whereas a reference must be bound to an existing object. If you try hard enough, you can bind a reference to `nullptr`, but this is [undefined](https://stackoverflow.com/questions/2397984/) and will not behave consistently.

# Reference

## Access type of refference

```cpp
//pass by reference, non-write-change access
void foo(const Type& element)

// pass by reference, write-change-able access
void foo(Type& element)

// move-able access
void foo(Type&& element)


```
