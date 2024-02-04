

# Use case
Check if this UART_id is constructed somewhere 
	If NOT `construct_instance(idx, id, config);`
	If Constructed check if Config is the same
		If so return a weak handle(weak pointer) to that instance

## Shared pointer and weak pointer

At first I want to create a boolean static array to keep track of shared pointer point to memory but we already have `weak pointer` which keeping track much more efficiently
When ever we created a handle_t We just assign that handle_t (which is a shared pointer) to weak_handles array.
```cpp
using handle_t = std::shared_ptr<Impl>;

auto return_handle = handle_t{ new (p) Impl(std::forward<Args>(args)...),
						[](Impl* _p){std::destroy_at(_p);} };

weak_handles[idx] = return_handle;



static std::array<weak_handle_t, N_INSTANCES> weak_handles;

```



## Memory align
hw_iface.h
```cpp
alignas(std::alignment_of_v<Impl>) static std::byte instance_memory[N_INSTANCES * sizeof(Impl)];
static std::array<weak_handle_t, N_INSTANCES> instance_handles;
```

`std::byte`: to hold a raw byte in memory without the assumption that it's a character. Can see that in [cppreference](http://en.cppreference.com/w/cpp/types/byte).
> Like char and unsigned char, it can be used to access raw memory occupied by other objects (object representation), but unlike those types, it is not a character type and is not an arithmetic type.

Remember that C++ is a strongly typed language in the interest of safety (so implicit conversions are restricted in many cases). Meaning: If an implicit conversion from `byte` to `char` was possible, it would defeat the purpose.

To use it, you have to cast it whenever you want to make an assignment to it:
```cpp
std::byte x = (std::byte)10;
std::byte y = (std::byte)'a';
std::cout << (int)x << std::endl;
std::cout << (char)y << std::endl;
```

`alignas(std::alignment_of_v<Impl>)`: this to make sure the instance_memory align with memory of `Impl`. Just to be sure. This from `#include<type_traits>`



## construct_instance()
hw_iface.h
```cpp
using handle_t = std::shared_ptr<Impl>;
auto ret = handle_t{ new (p) Impl(std::forward<Args>(args)...), [](Impl* _p){std::destroy_at(_p);} };
```

Let's break it down:

- `ret = new (p) Impl`: This is a form of `new` that allows you to construct an object at a specific memory location that you provide. The new `Impl` object is being constructed at the memory location pointed to by `p`. 
- The arguments for the `Impl` constructor are forwarded using perfect forwarding, which allows the constructor arguments to be perfectly forwarded to the `Impl` constructor [geeksforgeeks.org](https://www.geeksforgeeks.org/placement-new-operator-cpp/).

- `[](Impl* _p){std::destroy_at(_p);}`: This is a lambda function that serves as a custom deleter for the `shared pointer`. 
- `[](Impl* _p){std::destroy_at(_p);}` only destroy the pointer not the memory
