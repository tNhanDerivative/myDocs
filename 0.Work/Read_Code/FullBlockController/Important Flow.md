Here's how the `onDriveBlocked()` function is called:

1. When a new drive is inserted, the Oesis SDK (a third-party library used for drive management) detects the insertion event and generates a corresponding event.
    
2. In the `FullBlockController::init()` function, the `registerOesisEvent()` function is called, which registers event handlers with the Oesis SDK. One of the registered event handlers is an instance of the `InsertEvent` class, which is responsible for handling drive insertion events.
    
3. When a drive insertion event occurs, the `InsertEvent` instance's `OnEvent()` function is called by the Oesis SDK. Inside this function, the `FullBlockController::onDriveBlocked()` function is called, passing the event data as a JSON object.
    
4. The `onDriveBlocked()` function then processes the event data, retrieves information about the inserted drive, and performs various actions based on the drive's properties and the current configuration settings. These actions may include blocking or unblocking the drive, displaying notifications, and sending reports to the server.