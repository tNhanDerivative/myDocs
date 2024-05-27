
# Sample code

```cpp
class Singleton{
public:
    /**
     * Singletons should not be cloneable. 
     * to prevent lack of reference symbol `&` like this
     * Singleton a_reference = Singleton::get_instance;
     */
    Singleton(Singleton &other) = delete;
    /**
     * Singletons should not be assignable.
     */
    void operator=(const Singleton &) = delete;


	static Singleton& create_instance(int x);
	static Singleton& get_instance();
private:
	int _x;
	Singleton(int x): x(_x) {}
	
	static Singleton* only_instance
	
}

static Singleton* Singleton::only_instance = nullptr;
```

```cpp
#include<iostream>

/**
 * Static methods should be defined outside the class.
 */
Singleton *Singleton::create_instance(int x)
{
    /**
     * This is a safer way to create an instance. instance = new Singleton is
     * dangeruous in case two instance threads wants to access at the same time
     */
    if(singleton_==nullptr){
        only_instance = new Singleton(x);
    }else{
	    std::cout << "Singleton already created";
	}
	
	return only_instance;    
}

Singleton *Singleton::get_instance(){    return only_instance;}

```

Make constructor private
Creat a static instance; initialize the instance like any static member
Create method to access this Single object statically `get_instance`
