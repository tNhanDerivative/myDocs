
You'll have to use the method `c_str()` to get the C string version.

```cpp
std::string str = "string";
const char *cstr = str.c_str();
```

`.c_str()` returns a `const char *`. If you want a non-const `char *`, use `.data()`:

```cpp
std::string str = "string";
char *cstr = str.data();
```


```cpp
	char username[UNLEN + 1];
    DWORD username_len = UNLEN + 1;
    GetUserNameA(username, &username_len);
    std::string serverConfigTXTPath = "C:\\Users\\" + static_cast<std::string>(username) +
                                "\\Downloads\\severConfig.txt";
    configResponse = FileUtil::FileContentsAsStringA( serverConfigTXTPath.c_str());
```