create solution space
create blank project
Right click => property
trong General
```
Output Directory: $(SolutionDir)build\
Intermediate: $(SolutionDir)build\try_user_mode_intermediate\
C++ standard: C++ 20
```

## Linker
project cần dùng ioctl cần quyền admin
Linker ==> All option (type UAC, chinh)
Execution Level: requireAdministrator (/level='requireAdministrator')
## Code Generation
`Runtime Library: Multithread Debug`
Compiler will embed all dependencies into our binary so it's gonna run able on virtual machine 