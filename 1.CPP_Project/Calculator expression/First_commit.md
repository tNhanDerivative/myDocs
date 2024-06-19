

# First commit

```cpp
#include <iostream>
#include <cmath>

using std::cout, std::endl, std::ostream;

struct Expression {
    virtual ~Expression() {}
    virtual void print(ostream& out) const = 0;
    virtual double eval() const = 0;
    friend ostream& operator<<(ostream& out, const Expression& e){
		e.print(out);
		return out;
	}
};


template<char Op>
class BinaryExpression: public Expression {
    Expression *e1, *e2;
    virtual double evalImpl(double d1, double d2) const = 0;
public:
    BinaryExpression(Expression* e1, Expression* e2): e1(e1), e2(e2) {}
    
    ~BinaryExpression() override {
        delete e1;
        delete e2;
    }
    
    void print(ostream& out) const override {
        out << '(' << *e1 << ' ' << Op << ' ' << *e2 << ')';
    }
    
    double eval() const override { 
        return evalImpl(e1->eval(), e2->eval());
    }    
};


class Number: public Expression {
    double num;
public:
    Number(double num): num(num) {}
    void print(ostream& out) const override {
        if (num < 0) out << '(' << num << ')';
        else out << num;
    }
    double eval() const override { return num; }
};


struct Sum: public BinaryExpression<'+'> {
    using BinaryExpression<'+'>::BinaryExpression;
    double evalImpl(double d1, double d2) const override {
        return d1 + d2;
    }
};

struct Exp: public BinaryExpression<'^'> {
    using BinaryExpression<'^'>::BinaryExpression;
    double evalImpl(double d1, double d2) const override {
        return std::pow(d1, d2);
    }
};

int main() {
    auto e = new Sum(
				    new Exp(new Number(3), new Number(2)), 
					new Number(-1)
					);
    cout << *e << " = " << e->eval() << endl;
    delete e;
}
```


# Explain class design

## Key concept
Expression có virtual function `virtual double eval() const = 0;`
BinaryExpression inherit from Expression implement eval()
```cpp
double eval() const override { 
	return evalImpl(e1->eval(), e2->eval());
}
```
eval call  `virtual double evalImpl(double d1, double d2) const = 0;`

Sum and Exp are concrete implementations of `BinaryExpression` for addition and exponentiation, respectively. They override `evalImpl` to provide the specific operation's logic.

## Resource management

When we use delete a pointer of a resource, It will call our custom destructors
And if the resource also manage another resource it also call it's destructors


## Abstract Base Class: `Expression`

- **Purpose**: Serves as the base class for all expressions. It defines the interface for evaluating and printing expressions.
- **Members**:
    - `virtual ~Expression()` ensures that derived classes can be safely deleted through a base class pointer.
    - `virtual void print(ostream& out) const = 0;` is a pure virtual function that must be implemented by derived classes to define how expressions are printed.
    - `virtual double eval() const = 0;` is another pure virtual function that must be implemented by derived classes to evaluate the expression to a numerical value.
    - `friend ostream& operator<<(ostream& out, const Expression& e);` declares `operator<<` as a friend function, allowing it to access the private members of `Expression` and its derived classes to print expressions.
    - 
### The use of a friend function for overloading the operator<<

*Access to Private Members*:  The `print()` method of the `Expression` class is declared as const, meaning it does not modify the state of the object it operates on. However, since it needs to access private members of derived classes (like Number) to perform its operation, it cannot be made public.

Polymorphic Behavior


### If declare operator<< outside of the class as a stand alone


```cpp
struct Expression {
    virtual ~Expression() {}
    virtual void print(ostream& out) const = 0;
    virtual double eval() const = 0;
    friend ostream& operator<<(ostream& out, const Expression& e);
};

ostream& operator<<(ostream& out, const Expression& e) {
    e.print(out);
    return out;
}
```
This friend function can't access to the private and protected members of `Expression` derived classes
This is crucial for implementing the stream insertion operator (`<<`) effectively, as it needs to invoke the `print` method, which is private in the base class but overridden in derived classes.

## Derived Classes
### `BinaryExpression`

- An abstract base class for binary expressions. It holds pointers to two expressions (`e1` and `e2`) and defines a virtual destructors to clean up dynamically allocated memory.
- `void print(ostream& out) const override;` prints the expression in infix notation.
- `double eval() const override;` evaluates the expression by applying the operation to the evaluated results of `e1` and `e2`.

### `Number`

- Represents a numeric expression. It overrides `print` to display the number (with parentheses around negative numbers) and `eval` to return the numeric value.



### `Sum` and `Exp`

- These are concrete implementations of `BinaryExpression` for addition and exponentiation, respectively. They override `evalImpl` to provide the specific operation's logic.

*Constructor*: inherited by keyword `using`








