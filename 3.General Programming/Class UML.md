
![[observe pattern.png]]

# Association
Mean class A will use class B
(pass B to A by value)

> In case of designing. It may mean we haven't sure it's Aggregation or Composition
Indicate class A may has a property has value, pointer or reference type B.
We will pass class B object to A but not sure if A handle lifetime of B; or have a reference of B. 

## Aggregation (quan hệ thu nạp)
Class A will use class B by reference or shared pointer (not own it, but borrow it)

```cpp
class Bar{}

class Foo{
	Bar _bar;
	Foo(cons &bar){
		_bar = bar;
	}
}
```

```cpp
class Bar{}

class Foo{
	Bar *_bar_ptr = new Bar;
}
```
When Foo dies Bar live on
## Composition
Class A will manage the lifetime of class B. 
Should pass in by smart pointer and owning
# Implementation
Is the relationship of a class implement an interface
# Inheritance
# Dependency