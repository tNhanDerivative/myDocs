
Project structure
```
Project/
├── Core/
│   └── ... source files ...
|
├── App/
│   └── ... source files ...
|
├── CMakeLists.txt
|
└── build/
    └── ... generated build files ...

```

how to
```bash
cd /path/to/your/project
mkdir build  
cmake -B build/default         # generate build files in the "build/" directory
cmake --build build/default    # Build project using the build system generated in the "build/" directory
```