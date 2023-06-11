https://realpython.com/python-modules-packages

## Python package
*Packages* allow for a hierarchical structuring of the module namespace using *dot notation*.
![package](package.PNG "package")  ![subpackages](subpackages.PNG "subpackages")

method 1: **dot notation** is used to separate **package** name from **subpackage** name:
```python
import pkg.sub_pkg1.mod1
pkg.sub_pkg1.mod1.foo()

from pkg.sub_pkg1 import mod2
mod2.bar()

from pkg.sub_pkg2.mod3 import baz
baz()

from pkg.sub_pkg2.mod4 import qux as grault
grault()
```

method 2: you can use a **relative import**, where `..` refers to the package one level up. From within `mod3.py`, which is in subpackage `sub_pkg2`,
```python
from .. import sub_pkg1
print(sub_pkg1)

from ..sub_pkg1.mod1 import foo
foo()
```
