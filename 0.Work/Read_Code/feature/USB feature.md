
# Design idea

`USBFeature` has a unique pointer `_impl` of type `USBModeBase` this own a USB Mode, (there is 2 USB mode Full Block and on File Access)
`USBFeature` use this `_impl` pointer to use start, stop, init, deinit method of USB Mode
```cpp
 private:
  std::unique_ptr<USBModeBase> _impl;
```
`USBModeBase` has 2 concrete class is `FullBlockController` and `FileAccessController` this have the actual `init()` `start()` `stop()` ...





