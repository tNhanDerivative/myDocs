## partial function
https://www.geeksforgeeks.org/partial-functions-python/

The `partial` function takes a callable (usually another function) and a series of arguments to be pre-filled in the new partial function.

```python
from functools import partial
 
# A normal function
def f(a, b, c, x):
    return 1000*a + 100*b + 10*c + x
 
# A partial function that calls f with
# a as 3, b as 1 and c as 4.
g = partial(f, 3, 1, 4)
 
# Calling g()
print(g(5))
```
Output: 3145