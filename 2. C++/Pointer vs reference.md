
# Source
[ github question](https://stackoverflow.com/questions/57483/what-are-the-differences-between-a-pointer-variable-and-a-reference-variable)

A pointer can be assigned `nullptr`, whereas a reference must be bound to an existing object. If you try hard enough, you can bind a reference to `nullptr`, but this is [undefined](https://stackoverflow.com/questions/2397984/) and will not behave consistently.

# Use referenceTo avoid object slicing

vector of pointer point to object same base class. Because the children class has different size.
We can also dynamic_cast pointer at runtime.
This also why we need dynamic_cast

#  Use pointer  to Achieve runtime polymorphism in a function

# Use pointer if we want to be clear about passing exact that thing
If we use reference, we might pass into a function want to copy by value.


