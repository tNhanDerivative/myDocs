

# Structure 

## Singleton
`DriveManager::I()` return a singleton instance
I think it doesn't need Dependency injection. It is dependency injection
```cpp
 private:
  static DriveManagerPtr _instance;
```

```cpp
DriveManagerPtr DriveManager::I() {
  std::lock_guard lock(_mtx);
  if (!_instance) {
    _instance = std::make_shared<DriveManager>();
  }
  return _instance;
}
```