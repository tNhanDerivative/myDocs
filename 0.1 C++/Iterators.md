[writing custom iterator](https://internalpointers.com/post/writing-custom-iterators-modern-cpp)
An iterator is an object that points to an element inside a container( vector, array, list)
An iterator is a simple class that provides a bunch of operators: begin(); end(); `++` used to access the element it points to
which make it very similar to a pointer and the arithmetic operations you can perform on it. In fact, iterators are a generalization of pointers.

Iterator

```cpp
#include <iterator>
#include <cstddef>

class Integers
{
public:
    struct Iterator 
    {
        using iterator_category = std::forward_iterator_tag;
        using difference_type   = std::ptrdiff_t;
        using value_type        = int;
        using pointer           = int*;
        using reference         = int&;

        Iterator(pointer ptr) : m_ptr(ptr) {}

        reference operator*() const { return *m_ptr; }
        pointer operator->() { return m_ptr; }
        Iterator& operator++() { m_ptr++; return *this; }  
        Iterator operator++(int) { Iterator tmp = *this; ++(*this); return tmp; }
        friend bool operator== (const Iterator& a, const Iterator& b) { return a.m_ptr == b.m_ptr; };
        friend bool operator!= (const Iterator& a, const Iterator& b) { return a.m_ptr != b.m_ptr; };  

    private:
        pointer m_ptr;
    };

    Iterator begin() { return Iterator(&m_data[0]); }
    Iterator end()   { return Iterator(&m_data[200]); }

private:
    int m_data[200];
};
```

To quickly read an Iterator like `std::filesystem::directory_iterator` you just need to 
>read Member types:  `value_type` which is the type of object inside container
[cpp reference directory_iterator](https://en.cppreference.com/w/cpp/filesystem/directory_iterator)