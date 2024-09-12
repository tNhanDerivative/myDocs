


```cpp
#include <catch2/catch.hpp>
#include <gmock/gmock.h>
#include "FullBlockController.h"

// Mock for agent_info::config
namespace agent_info {
    MOCK_FUNCTION(json11::Json, config, (const std::string&));
}

// Mock for Internal::WhiteListedProcesses
namespace {
namespace Internal {
    MOCK_FUNCTION(std::vector<std::wstring>, WhiteListedProcesses, ());
}
}

TEST_CASE("FullBlockController constructor", "[FullBlockController]") {
    // Setup mocks
    json11::Json mockConfig = json11::Json::object {
        {"exclude_cd_dvd", 1},
        {"exclude_phone", 0},
        {"blocked", 1},
        {"ignore_unformatted_drives", 1},
        {"exclude_disk_image", json11::Json::object {{"iso", 1}}}
    };

    std::vector<std::wstring> mockWhiteList = {L"process1.exe", L"process2.exe"};

    EXPECT_CALL(agent_info::config, ("config/media_security"))
        .WillOnce(testing::Return(mockConfig));

    EXPECT_CALL(Internal::WhiteListedProcesses, ())
        .WillOnce(testing::Return(mockWhiteList));

    // Create the FullBlockController
    FullBlockController controller;

    // Verify the constructor set the correct values
    REQUIRE(controller.excludeCD == true);
    REQUIRE(controller.excludeMobile == false);
    REQUIRE(controller.alwaysBlocked == true);
    REQUIRE(controller.excludeUnformatted == true);
    REQUIRE(controller.ignoreISO == true);

    // Verify the whitelist was set correctly
    // Note: We can't directly access _allowProcessNames as it's private,
    // so we'd need to add a getter method or make it protected for testing
    // REQUIRE(controller.getAllowProcessNames() == mockWhiteList);
}
```

This test does the following:

1. It mocks the `agent_info::config` and `Internal::WhiteListedProcesses` functions.
2. It sets up expectations for these mocked functions to return specific values.
3. It creates a `FullBlockController` instance.
4. It verifies that the constructor set the correct values for the public member variables.

Note that to fully test the private `_allowProcessNames` member, you'd need to either add a getter method to the `FullBlockController` class or make it protected for testing purposes.

