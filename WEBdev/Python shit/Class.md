**Python** is an Object-Oriented Programming Language, everything in python is related to objects, methods, and properties.

A class is a user-defined blueprint or a prototype, which we can use to create the objects of a class. The class is defined by using the class keyword.

## __init__()
[__init__()] is a built-in method for understanding the meaning of classes. Whenever the class is being initiated, a method namely __init__() is always executed. An __init__() method is used to assign the values to object properties or to perform the other method that is required to complete when the object is created.
```python
# create a Geeksforgeeks class
class Geeksforgeeks:
	# constructor method
	def __init__(self):
		# object attributes
		self.course = "Campus preparation"
		self.duration = "2 months"
		# define a show method
		# for printing the content
	def show(self):
		print("Course:", self.course)
		print("Duration:", self.duration)

# create Geeksforgeeks
# class object
outer = Geeksforgeeks()
# method calling
outer.show()
```

Ouput:
>Course : Campus Preparation
>Duration : As per your schedule

## Inner Class
A class defined in another class is known as an **inner class** or nested class.
Một object được tạo ra bởi inner Class tất nhiên có thể được truy cập bởi object tạo ra bởi Parent Class

### Tại sao dùng Inner Class
Suppose we have two classes remote and battery. Every remote needs a battery but a battery without a remote won’t be used. So, we make the Battery an inner class to the Remote.
Hence, Hiding the code is another good feature of the inner class. By using the inner class, we can easily understand the classes because the classes are closely related. We do not need to search for classes in the whole code, they all are almost together.

### Inheritance of Inner Class
https://www.geeksforgeeks.org/inheritance-in-python-inner-class/?ref=lbp
