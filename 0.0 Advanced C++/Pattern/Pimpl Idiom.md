
# tutorial
[youtube tutorial](https://www.youtube.com/watch?v=8V2Uz6zYoPQ&t=365s)
# forward declaration

But in case of `impl` class was in a namespace you need to forward declare namespace too
```cpp
namespace owc::hid {
  class HIDController;
}
```
Read this link [c++ - How to forward declare a class which is in a namespace - Stack Overflow](https://stackoverflow.com/questions/19001700/how-to-forward-declare-a-class-which-is-in-a-namespace)

![[ForwardDeclaration.png]]

# Destructor
![[destructorInCPP.png]]
Attention we have to had a destructor. This is to deal with what part of the compiler generated the code problem.
The cpp file `include` the `impl` class header which has the information about the `impl` class destructor even whether the `impl` class has a destructor
Without that the compiler generated destructor which would have been in the header file wouldn't have been able to generated. For that reason we have to declare a destructor and put it in the cpp file even though it has no body.

# Move constructor
```cpp
Account4::Account4(Account4&& otherAccount):pImpl(std::move(otherAccount.pImpl))
```

# Move assignment opperator
![[PimplMoveAsignmentOperator.png]]



# Avoid Manual Memory Management 
- some rules to live by as a "modern c++" developer 
    + avoid manual memory management 
    + use lambdas 
    + use standard containers 
    + embrace move semantics 
    + follow style rules 
    + consider the PIMPL idiom 
    
- manual memory management 
    + whatever you new you must delete, new[] then you must delete[] 
    + first step: writing destructor, typically cause copy problems 
    + second writing copy constructors, copy assignment operators
    the rule of 3 
    + requires you to explicitly decide about ownership 
    A.SetToPointer(b);
    who is going to delete b and when? 

- some simple rules 
    + if you're typing delete, you're doing it wrong, possible exemption if you are writing a library 

    + use local scope(stack semantics) as much as you can 
        * memeber variables that are instances not pointer 
        * smart containers of solid objects 
        * unless you have a good reason 
        
    + smart programmers use smart pointers 
        * shared_ptr, when last use goes out of scope, object on free store is deleted 
        * unique_ptr
        less overhead than shared_ptr 
        not copyable 
        make_uniques is missing but you can write and used it 
- member variable lifetime tied to the class 
    + old school, destrctor of the class deletes the object 
    + demo 


```cpp
class Resource 
{
private:
    std::string name;
public:
    Resource(std::string n);
    Resource(const Resource& r);
    Resource& operator=(const Resource& r)
    ~Resource(void);
    std::string GetName() const{return name;}
};
```

```cpp
Resource::Resource(string n):name(n)
{
    cout << "constructing" << name << endl;
}

Resource::Resource(const Resource& r):name(r.name)
{
    cout << "copy constructing" << name << endl;
}

Resource& Resource::operator=(const Resource& r)
{
    name = r.GetName();
    cout << "copy assigning " << name << endl;
    return *this;
}

Resource::~Resource(void)
{
}
```

```cpp
class Person()
{
private:
    Resource pResource;
public:
    Person() pResource("");
    void SetResource(string n)
    
        Resource newResource(n);
        pResource = newResource;
    }
}

use unique_ptr, unique_ptr.reset(value);

use shared_ptr, use make_shared<ResourceType>(value);
```


- observing other object , observe/access something without impact on its lifetime 
    + if the real object is in a shred_ptr, use a weak_ptr, can ask it whether the real object is killed
    
    + if the real object is solid use a raw pointer from & operator or a this with care, can check if pointer is nullptr not a complete check 

# use lambda 
# use standard containers 
# use standard algorithms 
# embrace move 
# follow style rules 
    + default paramters 
    + use nullptr 
    + no magic numbers 
    + get const from start 
    + treat warnings as errors 

# consider the PImpl idiom 
- c++ has header files 
    + separates the implementation(.cpp) from the definition (.h)
        * essentially, metadata in plain text form 
    + as projects get large, compiler take longer 
    + often many files include the same header 
    + even across projects 
    
- demo header changes 
#pragma once 
#include "*.h"
...

- forward declaration 
    + don't always need to #include entire class definition 
    class X;
    
    + as long as your code desn't rely on details on X, this is fine 
    can't have a solid x member variable 
    can't call any methods or access any public varibles 
    
    + so how can this help 
- PImpl is, stands for pointer-implementation, sometimes called compiler firewall
    + the class everyone uses has 
    one private variable, a pointer to the implementation class 
    public functions with same signatures as before, each calls the corresponding function in the implantation class 
    
    + now you can change the private part of the Impl class, nothing else need to recompile
    
- basic structure, the idiom of Pimpl is implemented with raw pionter 
```cpp
class AccountImpl;                          
                                            
class Account                               
{                                           
private:                                        
    AccountImpl* pImpl;                     }
public:
    int Any(int bal, Customer* cust);
};
```

```cpp
#include "Account.h"
Account::Any(int bal, Customer* cust)
{
	return pImpl->Any(bal, cust);
}
```

    
- demo PImpl with unique_ptr, will keep less code 
```cpp
std::unique_ptr<AccountImp> pImpl
```

- struct with unique_ptr 
```cpp
class AccountImpl;
class Account 
{
private:
    std::unique_ptr<AccountImpl> pImpl;
    
public:
    int Any(int bal, Customer* cust);
    ~Account();
    Account(Account&& otherAccount);
    Account4 opertor=(Account&& otherAccount);
};

```

```cpp
#include "Account.h"
#include "AccountImpl.h"
int Account::Any(int bal, Customer* cust)
{
    return pImpl->Any(bal, cust);
}

Account::~Account(){}

Account::Account(Account&& otherAccount):pImpl(std::move(otherAccount.pImpl)){}

Account& Account::operator=(Account&& otherAccount)
{
    pImpl = std::move(otherAccount.Impl);
    return *this;
}
```



- When to set PImpl 
    + you need to have long time to compile 
    + when there is one particular class whose header is included many places 
    first see if forward declarations would help you 
    
    + wjem tje class is already noncopyable 
    or at least isn't being copied in any existing code 
    prepared to write a meaningful deep cpy so existing code still works 
    
    + public interface stable, private is not stable, the private is volatile 

    
# stop writing c with classes 
- introduce how to transform old school c with class code to modern style 










