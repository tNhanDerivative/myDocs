
# Const_cast
When we need to call some 3'rd party lib where it take variable/object as non-const but not changing value of that variable/object
```cpp
const int x = 20;
const int *px = &x;
thirdPartyLib(const_cast<int*>(px));
```

You are not allowed to `const_cast` and then modify variables that are actually `const`. This results in undefined behavior. `const_cast` is used to remove the const-ness from references and pointers that ultimately refer to something that is not `const`

# dynamic_cast
Safely converts pointers and references to classes up, down, and sideways along the inheritance hierarchy.
At run-time
Need at least 1 virtual function in base class

Notes:
- A downcast can also be performed with static_cast, which avoids the cost of the runtime check, but it's only safe if the program can guarantee (through some other logic) that the object pointed to by expression is definitely `Derived`.

- Some forms of dynamic_cast rely on Run-time Type Identification(RTTI), that is, information about each polymorphic class in the compiled program. Compilers typically have options to disable the inclusion of this information.


```cpp
// C++ program demonstrate if there
// is no virtual function used in
// the Base class
#include <iostream>
using namespace std;

// Base class declaration
class Base {
	virtual void print(){ cout << "Base" << endl; }
};

// Derived Class 1 declaration
class Derived1 : public Base {
	void print(){ cout << "Derived1" << endl; }
};

// Derived class 2 declaration
class Derived2 : public Base {
	void print(){ cout << "Derived2" << endl; }
};

// Driver Code
int main()
{
	Derived1 d1;

	// Base class pointer hold Derived1
	// class object
	Base* bp = dynamic_cast<Base*>(&d1);

	// Dynamic casting
	Derived2* dp2 = dynamic_cast<Derived2*>(bp);
	if (dp2 == nullptr)
		cout << "null" << endl;

	return 0;
}

```