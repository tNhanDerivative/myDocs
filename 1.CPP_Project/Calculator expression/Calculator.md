


# Requirement

```cpp
int main() {
    auto e = new Sum(
                    new Exp(new Number(3), new Number(2)), 
                    new Number(-1)
                    );

                    
    cout << *e << " = " << e->eval() << endl;
    delete e;
}
```

We want to print math expression like this: ((3 ^ 2) + (-1)) = 8

    ## why use new
Because we can't know the length of the expression string. But later when we keep it as a resource member like a pointer. Maybe we will not have to use `new` anymore.




