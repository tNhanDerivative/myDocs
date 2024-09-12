
2. **Generate Build Files**: Open a terminal or command prompt, navigate to your project directory, and create a separate directory for the build files. This is known as an out-of-source build, which is recommended practice. For example, if your project directory is named `my_project`, you would do:

`cd my_project mkdir build cd build`

[

](https://www.phind.com/search?cache=vrmdlt4h8ws1ysjl58k75wej)

3. **Run CMake**: Still within the `build` directory, run CMake to generate the build system files. Assuming you have CMake installed and accessible from your PATH, execute:

`cmake..`

[

](https://www.phind.com/search?cache=vrmdlt4h8ws1ysjl58k75wej)

This command tells CMake to look for a `CMakeLists.txt` file in the parent directory (denoted by `..`) and generate the necessary build files for your project.

4. **Build Your Project**: After successfully generating the build files, you can compile your project. If you're using `make`, simply run:

`make`



This command compiles your project according to the instructions in the generated build files and produces an executable named `MyProject` (or whatever you named your project in the `CMakeLists.txt`).

5. **Run Your Executable**: Finally, you can run your compiled executable. On Linux or macOS, use:

`./MyProject`