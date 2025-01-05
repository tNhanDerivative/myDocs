
# only have 2 file main.cpp and mylib.hpp in the same folder

```sh
g++ main.cpp -o myprogram
```

Remember, the `.hpp` file (`mylib.hpp`) is only needed during the compilation of any `.cpp` file that includes it. Once the `.cpp` file is compiled, the header file's content is integrated into the binary, so you don't need to compile the header file itself.

# 3 file main.cpp, mylib.cpp, mylib.hpp

Compile 2 target into object file
compile both `mylib.cpp` and `main.cpp` into object files.
This step translates the source code into machine code but does not link them together. You can do this using the `g++` command with the `-c` flag, which compiles the source files into object files without linking.
```sh
g++ -c mylib.cpp -o mylib.o
g++ -c main.cpp -o main.o
```

**Link the object files**: After compiling the `.cpp` files into object files, you need to link these object files together to create the final executable. This step resolves symbols across different translation units and creates the executable file.
```sh
g++ mylib.o main.o -o myprogram
```

Alternatively, you can combine the compilation and linking steps into a single command by specifying all `.cpp` files and using the `-o` option to specify the output executable name. This approach is simpler and more straightforward for small projects or quick tests.
```sh
g++ main.cpp mylib.cpp -o myprogram
```