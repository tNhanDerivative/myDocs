# VI_API
- This code defines a special label (macro) called VI_API that helps control how functions and classes are shared between different parts of a program

- Automatically choose the right system API to import, export DLL based on what compiler is being used.

```cpp
#if ON_VI_ENGINE
	#if DYNAMIC_BUILD
		#ifdef _MSC_VER
			#define VI_API __declspec(dllexport)
		#else
			#define VI_API __attribute__((visibility("default")))
		#endif
	#else
		#define VI_API
	#endif
#else
	#if DYNAMIC_IMPORT
		#ifdef _MSC_VER
			#define VI_API __declspec(dllimport)
		#else
			#define VI_API
		#endif
	#else
		#define VI_API
	#endif
#endif
```


1. Whether we're building the engine (ON_VI_ENGINE) or using it
	- When building the engine we export
	- When using the engine we import
	
2. Whether we're building a dynamic library (DYNAMIC_BUILD) or importing one (DYNAMIC_IMPORT)
Based on this in CMake
```cmake
target_compile_definitions(${PROJECT_NAME} PUBLIC ON_VI_ENGINE DYNAMIC_LIB=0 DYNAMIC_BUILD=0)
```

- Dynamic library build (`DYNAMIC_BUILD`)
	- For Microsoft Visual C++ compiler (`_MSC_VER`), it uses `__declspec(dllexport)`
	- For other compilers (like GCC/Clang), it uses `__attribute__((visibility("default")))`
- Static library build
	- `VI_API` is defined as empty