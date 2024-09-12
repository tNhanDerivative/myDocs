

# Google test and Gmock
```cpp
#include <gtest/gtest.h>
#include <gmock/gmock.h>
#include "FullBlockController.h"

class MockUSBModeBase : public USBModeBase {
public:
    MOCK_METHOD(void, disable, (), (override));
};

class TestableFullBlockController : public FullBlockController {
public:
    MOCK_METHOD(void, deleteFeatureRegKeyChild, (), (override));
    MockUSBModeBase& getMockUSBModeBase() { return mockBase; }

private:
    MockUSBModeBase mockBase;
};

TEST(FullBlockControllerTest, DisableTest) {
    TestableFullBlockController controller;

    EXPECT_CALL(controller, deleteFeatureRegKeyChild())
        .Times(1);
    EXPECT_CALL(controller.getMockUSBModeBase(), disable())
        .Times(1);

    controller.disable();
}
```

This test does the following:

1. It creates a MockUSBModeBase class to mock the USBModeBase::disable method.
2. It creates a TestableFullBlockController class that inherits from FullBlockController, allowing us to mock the deleteFeatureRegKeyChild method and access the mocked USBModeBase.
3. It sets up expectations for both deleteFeatureRegKeyChild and USBModeBase::disable to be called exactly once.
4. It calls the disable() method on the controller.
5. Google Test will automatically verify that the expectations are met when the test completes.

This approach provides a more robust way to test the disable() method, ensuring that both deleteFeatureRegKeyChild and the parent class's disable method are called as expected.

# catch2 and gmock
```cpp
#include <catch2/catch.hpp>
#include <gmock/gmock.h>
#include "FullBlockController.h"

class MockUSBModeBase : public USBModeBase {
public:
    MOCK_METHOD(void, disable, (), (override));
};

class TestableFullBlockController : public FullBlockController {
public:
    MOCK_METHOD(void, deleteFeatureRegKeyChild, (), ());
    MockUSBModeBase& getMockUSBModeBase() { return mockBase; }

protected:
    MockUSBModeBase mockBase;
};

TEST_CASE("FullBlockController::disable", "[FullBlockController]") {
    TestableFullBlockController controller;

    REQUIRE_CALL(controller, deleteFeatureRegKeyChild())
        .Times(1);
    REQUIRE_CALL(controller.getMockUSBModeBase(), disable())
        .Times(1);

    controller.disable();
}
```

This test combines Catch2's test structure with GMock's mocking capabilities. It creates a TestableFullBlockController class that allows us to mock the deleteFeatureRegKeyChild method and access the mocked USBModeBase. The test sets up expectations for both methods to be called once and then calls the disable() method. Catch2 and GMock will verify that these expectations are met when the test runs.