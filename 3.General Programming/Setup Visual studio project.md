
#VisualStudio 
# Setup property multiple project
[C++ Infrastructure](https://www.youtube.com/watch?v=v6KQ0fqwNdA&list=PLqCJpWy5FoheHDzaP3R1eDFDOOff5TtBA&index=2)

# Static analysis
[](https://www.youtube.com/watch?v=1Ulji28Me1M&list=PLqCJpWy5FoheHDzaP3R1eDFDOOff5TtBA&index=3)

# Add include directory to visual studio solution
Right Click on Solution -> Property -> Edit
![[solution_rightClick.png|180]] ![[VSEdit_include.png|700]]

new line -> Add new file path (the file path is relative to where the project file resign)
Use Macros to include Solution directory you can include file in *another project* like this
`#include < [project_name]/[file_name] >`
```cpp
#include<Core/Core.h>
```

![[add_include_folder.png|300]] ![[macros_directories.png|500]]

# Linker error
ERROR look like: ... Unresolve external

A project reference is for when you are writing both the application and the library. In this case, the reference in the .proj file is pointing to another .proj file. 
When you build your solution visual studios will automatically build projects referenced to ensure you have the latest version. 
Sometimes it won't build a project if the project is already built... rebuild-all forces all projects to be rebuilt even if they are already built.

Right click on project ->add >-> reference![[add_reference.png|500]]

Debug
![[turnoffOptimize.png]]