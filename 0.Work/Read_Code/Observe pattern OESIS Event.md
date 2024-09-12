
Khi cần biết Event Type này sẽ gọi function nào của Observer
click vào event đó đọc 
```cpp
virtual void eventMsg(const Json::object& eventObj) override
```
xem trong function đó gọi method tên gì  như ví dụ dưới đây Event type `Type::plugin` gọi `_observer->onDrivePlugin(std::move(event)`

```cpp
class PluginEvent : public OesisTypedEvent<OesisEventHandler::Type::plugin> {
 private:
  MediaDriveEventListener* _observer = nullptr;

 public:
  PluginEvent(MediaDriveEventListener* observer) : _observer{observer} {};
  virtual void eventMsg(const Json::object& eventObj) override {
    std::lock_guard lock(_observer->syncEventMtx);
    if (eventObj.empty()) return;
    Json event = eventObj;
    RMP_LOGGER_INFO("Received plugin event: {}", event.dump());
    if (event["event_type"] != OesisEventHandler::Type::plugin) {
      return;
    }
    if (_observer) _observer->onDrivePlugin(std::move(event));
  }
};
```