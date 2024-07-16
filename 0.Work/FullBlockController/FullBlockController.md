
# Structure pattern

## FullBlockController is a EventListener of OesisEvent (EventManager)

[Observer (refactoring.guru)](https://refactoring.guru/design-patterns/observer)
1. When a new drive is inserted, the Oesis SDK (a third-party library used for drive management) detects the insertion event and generates a corresponding event.
    
2. In the `FullBlockController::init()` function, the `registerOesisEvent()` function is called, which registers event handlers with the Oesis SDK. One of the registered event handlers is an instance of the `InsertEvent` class, which is responsible for handling drive insertion events.
    
3. When a drive insertion event occurs, the `InsertEvent` instance's `OnEvent()` function is called by the Oesis SDK. Inside this function, the `FullBlockController::onDriveBlocked()` function is called, passing the event data as a JSON object.
    
4. The `onDriveBlocked()` function then processes the event data, retrieves information about the inserted drive, and performs various actions based on the drive's properties and the current configuration settings. These actions may include blocking or unblocking the drive, displaying notifications, and sending reports to the server.
    

In summary, the `FullBlockController::onDriveBlocked()` function is called indirectly by the Oesis SDK when a new drive is inserted. The Oesis SDK generates an event, which is handled by the `InsertEvent` class, which in turn calls the `onDriveBlocked()` function to handle the drive insertion event according to the configured media security policies.

# property
`FullBlockController` have some Atomic variable to keep track of the config

# Method
## FullBlockController::registerOesisEvent()

```cpp
auto & mdproxy = MdproxyMaker::GetProxy(); //singleton Mdproxy

auto insertEvent = std::make_shared<InsertEvent>(this);  // Should change name to insertEventListenter
mdproxy.RegisterEvent(insertEvent);
_oesisEvent.push_back(insertEvent);
```
`class InsertEvent` constructor take a pointer type `MediaDriveEventListener*`
We can pass `FullBlockController` instance to the constructor of `InsertEvent` because `FullBlockController` inherit from `USBModeBase`, which is inherit from `MediaDriveEventListener`

## constructor()
constructor will call `auto cfgs = agent_info::config("config/media_security");` to get config
then extract it store into Atomic property

```cpp
FullBlockController::FullBlockController()
    : USBModeBase(MediaSecurityMode::AllBlock) {
  using namespace keywords;
  auto cfgs = agent_info::config("config/media_security");
  excludeCD = cfgs["exclude_cd_dvd"] == 1;
  
...

  _allowProcessNames = Internal::WhiteListedProcesses();
}
```

## Method: onServerConfigChanged
Checking if config changed. If changed we update Atomic variable and call OESIS API again
```cpp

void FullBlockController::onServerConfigChanged(const json11::Json& jold,
                                                const json11::Json& jnew) {
  if (jnew["enabled"].int_value() == 0) return;

  bool alwaysBlockFalse2True = jnew["blocked"] == 1 && jold["blocked"] == 0;
  bool updateIgnoreISO =
      jold["exclude_disk_image"]["iso"] != jnew["exclude_disk_image"]["iso"];
      
...


  if (updateIgnoreISO) {
    ignoreISO = jnew["exclude_disk_image"]["iso"] == 1;
    RMP_LOGGER_INFO("Ignore ISO file = {}", ignoreISO);
  }

  if (alwaysBlockFalse2True) {
    RMP_LOGGER_INFO("Always block a portable media has been enabled");
    alwaysBlocked = true;
    deleteFeatureRegKeyChild();
    blockAllPortableDrives();
  }

  if (updateExcludeUnformatted || updateIgnoreISO) {
    Oesis::EnableAutoBlocking(MdproxyMaker::GetProxy(), true,
                              excludeUnformatted, ignoreISO);
  }

  blockAllISO(ignoreISO);


  loadFeatureConfigFromServer();
  refreshDrives(DriveFilter::CDExInfo);
}
```

## method: onDriveBlocked(const Json& event)
After Oesis API called this method will be called
And this method will call `shouldBlockDrive(driveID)` to check if we should unblock those drive

## method: shouldBlockDrive(driveID)
check xem drive có trong white list ko. Check xem loại drive là gì tương ứng có flag ignore, exclude nó ko. 
Nếu có thì trả về false tức là ko nên block


# Dependency 
## DriveManager
FullBlockController depend on DriveManager to block drive and update properties of drive
`DriveManager::I()` return a singleton instance
`DriveManager::I()->block(DriveType::DVD, !excludeCD);`
`DriveManager::I()->updateProperties(*insertedDrive);`


Singleton hàm `DriveManager::I()` trả về private `_instance`

## OESIS API
`EnableAutoBlocking()` is a shared function from `MdproxyShared.h`

*Attention* It only Block new drive coming. Example if config change ignore ISO from true to  False, those drives 've already been mounted won't be blocked. 


`MdproxyShared.h`
```cpp
namespace Oesis
{

inline json11::Json::object EnableAutoBlocking(
    Mdproxy& detect, bool enable = true, bool ignoreUnformattedDrive = false, bool ignoreISO = false) {
  using namespace json11;
 
  Json::object request{
      {"input", Json::object{{"method", WAAPI_MID_DRIVER_AUTOBLOCK},
                             {"ignore_raw_file_system", ignoreUnformattedDrive},
                             {"ignore_google_file_stream", true},
                             {"ignore_vhd_vhdx_volumes", true},
                             {"enable", enable},
                             {"ignore_iso", ignoreISO}}}};

  Json::object out = detect.invokeBlocking(request);
  OESIS_INFO("WAAPI_MID_DRIVER_AUTOBLOCK IN: {} - OUT: {}",
             Json(request).dump(),
           Json(out).dump());

  return out;
}

}
```