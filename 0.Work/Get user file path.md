


In Opswat project
```cpp
pathAllUser = owc::ms::StringUtils::toString(
          expandEnvironmentStrings(L"%ALLUSERSPROFILE%")) +
          "\\.ssh\\";
pathUser = owc::ms::StringUtils::toString(
          expandEnvironmentStrings(L"%USERPROFILE%")) +
          "\\.ssh\\",
```

In C++ general

```cpp
// lib to use `getenv`
#include <cstdlib>  
char* userProfile = getenv("USERPROFILE");

if (!userProfile) { 
// Handle the case where the environment variable is not available 
}

// Convert the `char*` to a `std::string` if needed for further processing
std::string userProfileString(userProfile);
```