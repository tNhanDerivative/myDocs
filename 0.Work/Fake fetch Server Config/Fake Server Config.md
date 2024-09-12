Old solution 
in `ServerConfig.cpp` find method  `RetrieveServerConfig`

#  Future Solution
1. In `ServerConfig.cpp`, replace the `FetchServerConfig()` function with a new function that fetches your fake JSON. For example:

```cpp
bool CServerConfig::FetchCustomJSON()
{
    GearsLogger::getInstance()->logMessage("Fetching custom JSON...");



    std::string customJSONString = "{ \"key1\": \"value1\", \"key2\": 42 }"; 

// FileUtil is our code to read local file
// std::string customJSONString = FileUtil::FileContentsAsStringA("C:\Users\xanhd\Downloads\severConfig.txt")

    // Parse the JSON string
    auto customJSON = JsonParse(customJSONString);

    if (customJSON.is_object())
    {
        _config.set(std::move(customJSON));
        return true;
    }
    else
    {
        GearsLogger::getInstance()->logMessage("Error parsing custom JSON");
        return false;
    }
}
```

2. In the lambda function connected to `observableConfigHash()` in `CServerConfig::CServerConfig()` (lines 433-523 in `ServerConfig.cpp`), replace the call to `FetchServerConfig()` with a call to `FetchCustomJSON()`:

```cpp

// this is to get GearsLogger
_configHash.silentlyConnect([this](const std::string& newHash) {
    if (!newHash.empty())
    {
        GearsLogger::getInstance()->logMessage("Config hash has changed, fetch the custom JSON");
        if (FetchCustomJSON()) {
            // No need to update the config hash since it's not related to the server configuration
        }
    }
});
```



# Explain Observer pattern to observe config_hash
The code in `GearsConnection.h`, `GearsConnection.cpp`, `ServerConfig.h`, and `ServerConfig.cpp` uses the Observer pattern to observe changes in the `config_hash` and fetch the latest server configuration when the hash changes.

Here's how it works:

1. In `ServerConfig.h`, the `CServerConfig` class has an `ObservableConfigHash` member variable `_configHash` of type `maf::signal_slots::Observable<std::string>`. This is a signal-slot implementation that allows observing changes to a string value (in this case, the `config_hash`).
    
2. In the constructor of `CServerConfig` (`ServerConfig.cpp`), an instance of `ObservableConfigHash` is created, and a lambda function is connected to it using the `silentlyConnect` method:
    

```cpp
_configHash.silentlyConnect([this](const std::string& newHash) {
    // Code to handle the new config hash
});
```

This lambda function is called whenever the `config_hash` changes.

3. Inside the lambda function, the following happens:
    
    - If the `newHash` is not empty, it means the `config_hash` has changed.
    - The `FetchServerConfig()` function is called to fetch the latest server configuration from the server.
    - If `FetchServerConfig()` is successful, the `config_hash` is updated in `GearsInfo` using `GearsInfo::getInstance()->updateServerConfigKey(GEARS_KEY_USB, "media_security", newHash)`.
    - The `config_hash` is also added as a header to the keep-alive request using `GearsConnection::getInstance()->addKeepAliveRequestHeader("config_hash", newHash)`.
4. In `GearsConnection::sendKeepAlive()` (`GearsConnection.cpp`), if the server response contains a `config_hash` field, the following line notifies the `CServerConfig` instance about the new hash:
    

```cpp
CServerConfig::getInstance().observableConfigHash().set(configHash);
```

This triggers the lambda function connected to `_configHash` in the `CServerConfig` constructor, and the process of fetching the latest server configuration is initiated.

5. The `addKeepAliveRequestHeader` and `removeKeepAliveRequestHeader` methods in `GearsConnection` (`GearsConnection.h` and `GearsConnection.cpp`) are used to manage the `config_hash` header in the keep-alive requests. When a new `config_hash` is received, the header is added to subsequent keep-alive requests using `addKeepAliveRequestHeader`. This allows the server to identify the client's current configuration version.

In summary, the code uses the Observer pattern implemented by `maf::signal_slots::Observable` to observe changes in the `config_hash`. When a change is detected, the `CServerConfig` instance fetches the latest server configuration from the server and updates the `config_hash` in `GearsInfo` and the keep-alive request headers.