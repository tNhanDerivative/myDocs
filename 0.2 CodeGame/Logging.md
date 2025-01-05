---
aliases:
  - spdlog
  - Logging
  - dependency injection by Macro
---

## Macro
`LOG_WITH_DETAILS` là macro chung nhận vào các parameter cần thiết.
`CORE_LOG_[Trace/Info/Error]` là Macro để truyền sẵn các para vào `LOG_WITH_DETAILS`


```cpp
#define LOG_WITH_DETAILS(logger, level, ...) logger->log(spdlog::source_loc{__FILE__, __LINE__, SPDLOG_FUNCTION}, level, __VA_ARGS__)

#if _DEBUG
	#define CORE_LOG_TRACE(...) LOG_WITH_DETAILS(VIEngine::Logger::GetCoreLogger(), spdlog::level::trace, __VA_ARGS__)
	
#else
	#define CORE_LOG_TRACE(...)
#endif
```

Chú ý: Cái hay ở đây là khi dùng các macro này chúng ta không depend vào các method cũng như các type  của parameter truyền vào
=> Lowkey dependency injection =))))

