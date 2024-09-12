

# Todo


Trong `MDEUnmanagedMain.cpp` log ra command mà user nhập
Đợi finalize how to log

```cpp
	std::stringstream ss;

    // Append each argument to the stringstream
    for(int i = 1; i < argc; ++i) { // Start from 1 to skip the program name
        ss << argv[i] << ' '; // Append the argument followed by a space
    }
    std::string userCommand = "mde-service.exe " + ss.str();    // Convert the stringstream to a string

```





# Document
[Challenge and Response - Technical - GEARS - Confluence (atlassian.net)](https://opswat.atlassian.net/wiki/spaces/GEAR/pages/3188851112/Challenge+and+Response+-+Technical)

# Read code
trong file `TOTPHelper.cpp` tìm method `generateChallenge(const uint8_t& commandID)`
khi click chuột phải `find all reference` tìm thấy hàm `generateChallenge()` được gọi trong `registerEngineHandler()` trong `MDEUnmanagedEngine`

- command password có thể là `challenge response`, có thể là `admin password`.

Trong `MDEUnmanagedMain.cpp` là entry point của comand line
nếu command có param thì sẽ launch CLI
ko có argument sẽ launch MDE service 
```cpp
  if (params.arguments().size() == 0) {
    return ServiceBase::launchService(MDEUnmanagedService(LPTSTR(constants::service_name)));
  }
  auto ec = CLIHelper::executeCommand(params);
  auto msg = CLIHelper::errorString(ec); 
```






