---
aliases:
  - dynamic Library
  - 
---
# Overview dll
[Why use dll ](https://www.youtube.com/watch?v=eLSpda2f28c)
[Basic dll](https://www.youtube.com/watch?v=pLy69V2F_8M&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=53)

Separating application to module. Convenient reusing, sharing third party
Adding component to your app
Hot code reloading
Split project into smaller bit

# Choose dll at runtime
There are multiple versions of dll and you would want to load the newest one.
And you want your application DO NOT terminate when it can't find a dll, like without xbox controller dll we can just use keyboard

```cpp
handInput.xboxInput = LoadLibrary("xboxInput2_0.dll");
if(handInput.xboxInput==NULL){
	handInput.xboxInput = LoadLibrary("xboxInput1_0.dll");
}
if(handInput.xboxInput==NULL){
	handInput.xboxInput = 0;
}
```
or 
```cpp
	HMODULE dll = LoadLibraryA("dllProject.dll");

	if (dll)
	{
		foo_t *fooPointer = (foo_t *)GetProcAddress(dll, "foo");

		if (fooPointer){
		fooPointer();
		}
		else{std::cout << "Error loading the function\n";}

		FreeLibrary(dll); //unload the DLL.
	}
```

# How to create 
## Cmake
```cmake
add_library(${PROJECT_NAME} SHARED ${SRC_FILES})
```
## Tell compiler what to export
for window's MSVC compiler
```cpp
extern "C" __declspec(dllexport) void foo()
{
	std::cout << "hello from the dll!\n";
}
```

for other compiler
```cpp
__attribute__((visibility("default")))
```

Vẫn chưa hiểu đoạn dllimport này [trích từ](https://learn.microsoft.com/en-us/cpp/build/importing-into-an-application-using-declspec-dllimport?view=msvc-170)

> 	Using **`__declspec(dllimport)`** is optional on function declarations, but the compiler produces more efficient code if you use this keyword. However, you must use **`__declspec(dllimport)`** for the importing executable to access the DLL's public data symbols and objects. Note that the users of your DLL still need to link with an import library.

Often we don't write this into code. In [[Pre-compiled header]] we define a Macro helps control how functions and classes are shared between different parts of a program.


