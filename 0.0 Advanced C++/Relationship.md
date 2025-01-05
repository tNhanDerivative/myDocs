
# Association

Aggregation: Class A sử dụng class B nhưng ko sở hữu class B object
Ví dụ: Class A có 1 pointer hoặc reference kiểu B
share pointer: share ownership
Aggregation có thể có 1 chiều hoặc 2 chiều; 2 chiều cần cẩn thận quan cylic reference

Composition: Class A sở hữu class B object
ví dụ: Class A có 1 unique pointer kiểu B

### Reference

When you delete an object that is referenced by another object (using a reference), the behavior is undefined. The program will likely crash or exhibit unexpected behavior.

### Key Points to Consider

1. References are aliases for objects. When you delete the original object, you're essentially deleting the very thing the reference points to [1].

2. Attempting to access or manipulate memory after deletion leads to undefined behavior [1].

3. This situation often results in segmentation faults or crashes [1].

4. It's important to manage object lifetimes carefully when using references.

### Code Example

```cpp
class A {
    int value;
public:
    A(int v) : value(v) {}
};

class B {
private:
    const int& ref;  // Reference member
    
public:
    B(const A& a) : ref(a.value) {}  // Cannot reassign ref after initialization
};

int main() {
    A obj(42);
    B b(obj);  // b.ref is now an alias for obj.value
    
    // This will cause undefined behavior if uncommented
    // delete &obj;
    
    return 0;
}
```

In this example, `b.ref` is an alias for `obj.value`. Deleting `obj` would be equivalent to deleting part of `b`, which is invalid.

### Best Practices

1. Always ensure that objects referenced by other objects outlive them [2].

2. Use smart pointers or RAII techniques to manage object lifetimes [2].

3. Be cautious when passing objects by reference to functions or returning them from functions [2].

4. Consider using const references when you don't need to modify the referenced object [2].

5. If you need to break ties between objects, consider using pointers instead of references [2].

Remember, when dealing with C++ objects and their lifetimes, it's crucial to understand ownership and lifetime management to avoid such issues. Always ensure that objects are not deleted while still being referenced elsewhere in your program.





