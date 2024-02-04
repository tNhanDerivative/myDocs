
# Move semantic
[Move semantics](https://www.youtube.com/watch?v=Bt3zcJZIalk&t=2179s)

## ví dụ về vector
```cpp
template<typename T>
class vector {
public:
	void push_back(const T& element); // copy element into vector

	void push_back(T&& element); // move element into vector

}
```

như vậy vector có thể move hoặc copy tùy vào rvalue hay lvalue đọc
[[Pointer vs reference#Access type of refference]]

## Class move semantics

### Copy as fallback
- Nếu move semantic không dc cung cấp trong class, coppy semantics sẽ dc dùng.
- Ta có thể disable fallback này












# Perfect Forwarding
[[hware_iface#construct_instance()]]

It allows move semantics to be automatically applied, even when the source and the destination of a move are separated by intervening function calls. 
Common examples include constructors and setter functions that forward arguments they receive to the data members of the class they are initializing or setting, as well as standard library functions like make_shared. which “perfect-forwards” its arguments to the class constructor of whatever object the to-be-created shared_ptr is to point to