## Main source
### My code
[My code on github](https://github.com/tNhanDerivative/esp32_wifi_wrapper)

### video
[video code: 5:47](https://www.youtube.com/watch?v=KV0D8nrsBLg&list=PLnq7JUnBumAz5mxA1SRcEKz-z6Zhsp7cz&index=4)

### github
https://github.com/0015/ThatProject/blob/master/MESSAGE/Twilio/0_ESP32TTGO_FIRESTORE_SMS/Network.h


## Arduino IDE set up and how to create library
[[Arduino IDE set up]]

[How to create Arduino Lib](https://roboticsbackend.com/arduino-create-library/)

## Wifi event (Observer pattern)

https://www.upesy.com/blogs/tutorials/how-to-connect-wifi-acces-point-with-esp32#use-wifi-events-to-have-an-optimized-code

https://techtutorialsx.com/2019/09/01/esp32-wifi-events-station-got-ip-address/



```cpp
#ifndef Network_H_
#define Network_H_

#include <WiFi.h>
#include <Firebase_ESP_Client.h>

typedef enum {
  NETWORK_CONNECTED,
  NETWORK_DISCONNECTED,
  FIREBASE_CONNECTED,
  FIREBASE_DISCONNECTED
} Network_State_t;

class Network{
private:
  FirebaseData fbdo;
  FirebaseAuth auth;
  FirebaseConfig config;

  typedef void (*FuncPtrInt)(Network_State_t);

  void firebaseInit();
  friend void WiFiEventConnected(WiFiEvent_t event, WiFiEventInfo_t info);
  friend void WiFiEventGotIP(WiFiEvent_t event, WiFiEventInfo_t info);
  friend void WiFiEventDisconnected(WiFiEvent_t event, WiFiEventInfo_t info);
  friend void FirestoreTokenStatusCallback(TokenInfo info);

public:
  FuncPtrInt callBackEvent;
  Network();
  Network(FuncPtrInt f);
  void initWiFi();
  void firestoreDataUpdate(double temp, double humi);
};


#endif
```



## Friend member of class
