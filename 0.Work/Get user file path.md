


In Opswat project
```cpp
pathAllUser = owc::ms::StringUtils::toString(
          expandEnvironmentStrings(L"%ALLUSERSPROFILE%")) +
          "\\.ssh\\";
pathUser = owc::ms::StringUtils::toString(
          expandEnvironmentStrings(L"%USERPROFILE%")) +
          "\\.ssh\\",
```

In