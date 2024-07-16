
# FeatureManager
- Declares a signal `allFeaturesReady` to notify when all features are ready.
- Defines an abstract interface for installing and loading features, which must be implemented by derived classes.

# Shared
- Provide shared functionality for the feature management system.
- Interact with the Windows Registry to store and retrieve feature properties, such as whether a feature is enabled, installed, or required.
- Offer methods to get and set feature properties, as well as retrieve a list of feature descriptions.
- Define a logging callback function for logging purposes.

# Featuredefs.h
- Defines the `FeatureDesc` struct, which holds information about a feature, such as its name, installation status, enabled status, and whether it's required.

# FeatureIF.h
- Defines the `FeatureIF` interface, which all feature classes must implement.
- The interface includes methods for initializing, deinitializing, starting, stopping, disabling, uninstalling, checking if stoppable, force stopping, and getting the version of the feature.

- It also includes an `ObservableStatus` type, which is an observable object that can notify subscribers about the feature's status (ready, unready, or error).

- The `SimpleFeature` class is a basic implementation of the `FeatureIF` interface, providing default implementations for most methods.





The relationships between these files can be summarized as follows:

- `FeatureManager` and `Shared` provide the core functionality for managing features and interacting with the registry.
- `FeatureIF` defines the interface that all feature classes must implement.
- `FeatureDefs` contains common definitions and data structures used throughout the feature management system.
- `FeatureFactory` provides a factory function to create instances of feature classes based on their names.
- `OWCFeatureManager` is a derived class of `FeatureManager` that handles feature management specific to the OWC application, including initialization, enabling, disabling, and installation based on various conditions and configurations.
- `GeoLocationFeature` is a concrete implementation of the `FeatureIF` interface, responsible for handling geolocation-related functionality.

The `OWCFeatureManager` class uses the `FeatureFactory` to create instances of feature classes, such as `GeoLocationFeature`. It then manages the lifecycle of these features using the methods provided by the `FeatureManager` class and the `Shared` utility functions.



WAOnDemand.cpp (main file)
trong Messageloop call GearEngine->run()
GearEngine.cpp
OWCManager register event  serverConfig change trong ServerConfig.h

check trong usb feature sẽ owning 2 Object FullblockController và FileAccessController (OnAcess)

Trong FullBlockController  thông qua wraper  agent_info::config() gọi ServeConfig.h



GearAgentService process giong nhu mot server. Cac process khac se goi request toi nhan response 
MAF: communication between process or thread ví dụ func registerRequest(). Under nó sẽ use Namepipe (Window), Unix socket(MacOS)
3 way of communication:
Request-response
Property ( post object vào một buffer mà process khác đọc dc)
Signal 