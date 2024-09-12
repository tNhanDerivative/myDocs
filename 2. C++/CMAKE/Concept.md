
# Concept

## Target
`Executable target`: represents an independent program that is built from specified source files. 
Each executable typically has its own main function and entry point.

`Library target`  It is used to define a library target that can be linked to other targets. 
When you have common code that needs to be reused across multiple executables or libraries, you encapsulate this code into a library

## Linkage
- **Public Linkage (`PUBLIC`)**: Specifies that the libraries are required for the target and also exposes them to any targets that link to this target. This is useful for libraries that are essential to the functionality of the target and might also be needed by other targets.
- **Private Linkage (`PRIVATE`)**: Indicates that the libraries are required for the target but are not exposed to other targets. This is suitable for libraries that are internal to the target and not needed by consumers of the target.
- **Interface Linkage (`INTERFACE`)**: Used to specify libraries that are part of the target's interface, meaning they are required for the target but are intended to be used by other targets that link to this target. This is particularly useful for header-only libraries or libraries that provide interfaces for other targets.