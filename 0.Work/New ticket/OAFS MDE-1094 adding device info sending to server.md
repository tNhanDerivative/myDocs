Add device information in blocked event

# Check server receiving info

![[Severver_Action_Log.png]]
![[Server_Action_Log_1.png]]


# 
```cpp
#ifndef MDEUnmanaged
#include "USBReport.h"
#endif // MDEUnmanaged
```


Text for `FileAccessController::onDrivePlugin` for handle Mobile device using OESIS
```cpp
  auto sNotification =
      "Media drive " + insertedDrive->name +
      " is blocked according to the security policies. Product ID: " +
      insertedDrive->productID + "; Vendor name: " + insertedDrive->vendor +
      "; Instance path: " + insertedDrive->deviceInstance;
```

Text for `FileAccessController::onDriveAttached` for other device using inhouse filter Driver
```cpp
  auto sNotification =
      "Files on media drive " + drive.get_drive_name() +
      " are blocked until manually scanned, according to the security policies. Product ID: " +
      drive.get_productID() + "; Vendor name: " + drive.get_vendor() +
      "; Instance path: " + drive.get_device_instance();
```

send report
```cpp      
#ifndef MDEUnmanaged
  int time = static_cast<int>(std::time(nullptr));
  auto result =
      Json::object{{"event", 0}, {"start_time", time}, {"end_time", time}};
  sendUsbSecurityReport(
      Json::object{{"result", result},
                   {"action_result", 1},
                   {"explanation_result", std::move(sNotification)},
                   {"vault", Json::object{}}});
#endif  // MDEUnmanaged
```
