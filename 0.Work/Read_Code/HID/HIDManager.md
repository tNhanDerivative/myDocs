
# Crash
```cpp
bool HIDManager::enableMonitoring(MonitorMode mode)
```
this method create a thread. When you call this method the second time the service will crash.
>FIX: check if the thread exist
```cpp
  if (msgLoopThread_.joinable()) {
    return true;
  }
```




When the user locks the screen, the following behavior occurs:

1. In the HIDManagerImpl::isScreenLocked() function, the code checks if the screen is locked by enumerating Windows sessions and querying their state.
    
2. In the HIDManagerImpl::blockHIDByInsPath() function, if the screen is locked and the HIDManager is in Default mode, it doesn't immediately block new devices. Instead, it adds them to a waiting list (waitingForBlocked).
    
3. When the screen is unlocked (detected in the WM_WTSSESSION_CHANGE message handler in wndProc), the HIDManagerImpl::blockWaitingList() function is called. This function then blocks all the devices that were added to the waiting list while the screen was locked.
    
4. Additionally, when the screen is locked, the HIDManagerImpl::deviceChangedHandler() function still detects new device connections, but it doesn't immediately block them due to the behavior described in point 2.
    
5. The USBModeBase class, which uses the HIDManager, doesn't directly handle screen lock events, but it benefits from the HIDManager's behavior in such situations.



# Wnd
The `wndProc` function is a Windows message handling procedure defined in the `HIDManager.cpp` file. It's responsible for processing messages related to device changes and session events. Here's a breakdown of its functionality:

1. It retrieves the `HIDManagerImpl` instance associated with the window.
2. For `WM_DEVICECHANGE` messages:
	- It calls the `deviceChangedHandler` method to handle device insertion or removal events.
1. For `WM_WTSSESSION_CHANGE` messages:
	-  On session logon or unlock, it logs the event, rescans HIDs, and blocks devices in the waiting list.
	-  On session logoff or lock, it clears blocked devices and rescans HIDs.
1. The function is set as the window procedure for a hidden window created by the `HIDManager` to receive system messages about device and session changes.
This `wndProc` function is a crucial part of the HID monitoring system, allowing the `HIDManager` to react to system events and maintain an up-to-date state of connected HID devices.




