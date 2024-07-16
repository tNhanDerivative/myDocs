# 
- Define role of BLE device
-  Managing connection: provide protocol of how BLE device connect

# Connection of Peripheral & Central
**Peripheral:** Advertises its presence so central devices can establish a connection. Developer can set frequency.
After connecting, peripherals no longer broadcast data to other central devices and stay connected to the device that accepted connection request.

- Peripherals are low-power because they only have to send beacons periodically. Central devices are responsible for starting communication with peripherals.
- Your BLE mouse is an example of Peripheral 

**Centrall** When the central device wants to connect, it sends a request connection message to the peripheral device.
Request connection message contain: Frequency and the Channel the peripheral need to tune to.
If the peripheral device accepts the request from the central device, a connection is established.