
[video Smart pointer](https://www.youtube.com/watch?v=x_eHJmdGQ_4)
# Why using Heap memory and pointer
The point of using heap and pointer is to using a resource outside of the scope we created it

```cpp
class data{int num};

data getData(){
	data a ={2} // fix: data* a = new data{2}
	return &a
}

int main(){
	data* dat = getData;
	std::cout<< *dat<< std::endl;
	return 0;
}
```

# reset() method
1. Avoid accessing the old object after `reset()`: After calling `reset()`, the old object gets deleted if no other smart pointer is managing it. Therefore, avoid accessing the old object after calling `reset()` as it leads to undefined behavior.
2. Use `reset()` to replace the managed object: The `reset()` method provides an efficient way to replace the managed object without worrying about deallocating the memory of the old object. Hence, prefer using `reset()` instead of manual memory management.
```cpp
   // Creating a unique_ptr and initializing it with a new object.
   unique_ptr<int> myPtr(new int(10));
   // Replacing the managed object with a new one.
   myPtr.reset(new int(20));
```
    
3. Be aware of the effect of `reset()` on shared pointers: If the `reset()` method is called on a shared pointer, the reference count of the object is decreased by one. If the reference count becomes zero, the object is deleted.
    
4. Release ownership when needed: The `reset()` method can also be used to release the ownership of the managed object. This can be useful in situations where you want to hand over the ownership to another smart pointer or if you want to make sure that the object gets deleted at a certain point in the code.


## Shared pointer

[[hware_iface#construct_instance()]]

## Convert Unique ptr to Shared ptr
[Convert unique to share](https://www.nextptr.com/question/qa1257940863/converting-unique_ptr-to-shared_ptr-factory-function-example)

- This flexibility is particularly useful where the function may not know whether the caller wants exclusive or shared ownership of the returned object, like in Factory pattern.
- 

We can convert a unique_ptr to shared_ptr by move semantic
```cpp
std::unique_ptr<std::string> unique = std::make_unique<std::string>("test");
std::shared_ptr<std::string> shared = std::move(unique);
```

The conversion is possible through an `_std::shared_ptr<T>` constructor that takes an rvalue reference of `_std::unique_ptr<Y, Deleter>` type and  moves it
So
```cpp
//'foo()' returns a unique_ptr
std::unique_ptr<std::string> foo();

//from afunction return, a rvalue reference of unique_ptr, 
//Temporary is moved.
std::shared_ptr<std::string> sp1 = foo(); // OK
```


## Weak pointer

[cpp reference](https://en.cppreference.com/w/cpp/memory/weak_ptr)
- You must use a `weak_pointer` to pass ownership to your `newly created shared_pointer`. Because std::weak_ptr.lock() will return an empty object if object is destroyed

`(auto shared_p = weak_p.lock())` will have an empty == false just like when you `if(pointer)`
```cpp
{
    if (auto shared_p = weak_p.lock())
        std::cout << "\tobserve() is able to lock weak_ptr<>, value=" << *p << '\n';
    else
        std::cout << "\tobserve() is unable to lock weak_ptr<>\n";
}
```


### Use case:
Real use case: [[hware_iface#Shared pointer and weak pointer]]
`std::weak_ptr` is a smart pointer that holds a non-owning ("weak") reference to an object that is managed by std::shared_ptr . 
It must be converted to std::shared_ptr in order to access the referenced object.



### Circular reference

- Break reference cycles formed by objects managed by `std::shared_ptr`. 
If such cycle is orphaned (i.e., there are no outside shared pointers into the cycle), the shared_ptr reference counts cannot reach zero and the memory is leaked. 
```cpp
class Father
{
    char id;
    std::weak_ptr<Son> mySon;          // if this is a shared pointer
public:
    Father(char id) : id{id} {}
    ~Node() { std::cout << "Father gone" << std::endl; }
    /*...*/
    void makeSon(const std::shared_ptr<Son> s) { mySon = s; }
};


class Son
{
    char id;
    std::weak_ptr<Father> myFather;      // if this is a shared pointer
public:
    Son(char id) : id{id} {}
    ~Son() { std::cout << "Son gone" << std::endl; }
    /*...*/
    void findFather(const std::shared_ptr<Father> f) { myFather = f; }
};

```