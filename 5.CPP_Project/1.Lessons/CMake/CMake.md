
# Concept
`Executable target`: represents an independent program that is built from specified source files.

`add_executable(${TARGET} ${SOURCES})`: function in CMake is used to define Executable targets within a project.  The function takes the name of the executable as its first argument, followed by a list of source files that are used to build the executable.
The `add_executable()` function can be used multiple times for the same target, allowing source files to be spread across different directories. CMake concatenates these source files in the order they are listed, which is particularly useful when your project's source files are not all located in one directory

`target_include_directories(HelloAppBinary ${CMAKE_CURRENT_SOURCE_DIR}/include)`

`Library target`