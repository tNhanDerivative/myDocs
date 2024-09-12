
## Access type of refference

```cpp
//pass by reference, non-write-change access
void foo(const Type& element)

// pass by reference, write-change-able access
void foo(Type& element)

// move-able access
void foo(Type&& element)


```