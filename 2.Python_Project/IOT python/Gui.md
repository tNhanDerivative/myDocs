## Install tkinter
```bash
sudo dnf install python3-tkinter
```

sau đó import trực tiếp từ file ko cần phải pip install


## Callable object

```python
from typing import Callable

class IOTGui(tk.Tk):  
	def __init__(  
	self, power_toggle_fn: Callable[[bool], None]  
	) -> None:

```

Argument `power_toggle_fn` that must be a callable object, and returns an `None`. 
The callable object must take an `boolean` as its first argument and return an `None`

# Chú ý 
Đây là ví dụ tuyệt nhất cho Inheritance của class
#class/inheritance #OOP/Inheritance

nhất là `super().__init__()`

```python
class IOTGui(tk.Tk):
    def __init__( self) -> None:
        super().__init__()
        self.title("IOT App")
        self.geometry("400x250+300+300")
```