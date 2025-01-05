# Overview
Đọc [[Factory pattern]]
Application có 1 con trỏ để sử dụng `NativeWindow`
Khi `Application.h` include `WindowPlatform.h` (factory of Native Window) Application không cần biết về `GLFWPlatformWindow`

Application.h
```cpp
#include "WindowPlatform.h"
class Application{
	...
private:
	...
	std::unique_ptr<NativeWindow> mNativeWindow;
}
```

Application.cpp
```cpp
Application::Application(const ApplicationConfiguration& config) : mConfig(config), mEventDispatcher() {
		mNativeWindow.reset(WindowPlatform::Create(config.WindowSpec));
}
```
# Implement 


`WindowPlatform.h`
```cpp
#pragma once
#include"Window.h"

namespace VIEngine {
	class WindowPlatform {
	public:
		static NativeWindow* Create(EWindowPlatformSpec spec);
	private:
		WindowPlatform() = default;
		~WindowPlatform() = default;
		WindowPlatform(WindowPlatform&) = default;
	};
}
```

Window.h
```cpp
#pragma once

enum class EWindowPlatformSpec {
	GLFW,
	SDL,
	None
};

class EventDispatcher;

struct WindowData {
	int32_t Width, Height;
	EventDispatcher* Dispatcher;
};

class NativeWindow {
public:
	virtual ~NativeWindow() = default;
	virtual bool Init(const struct ApplicationConfiguration&, EventDispatcher*) = 0;
	virtual void Shutdown() = 0;
	virtual void Swapbuffers() = 0;
	virtual void PollsEvent() = 0;
	virtual bool ShouldClose() = 0;
protected:
	NativeWindow() = default;
	NativeWindow(NativeWindow&) = default;
};

```


`WindowPlatform.cpp`
```cpp
#include"WindowPlatform.h"
#include"GLFWPlatformWindow.h"
#include"pch.h"

namespace VIEngine {
	NativeWindow* WindowPlatform::Create(EWindowPlatformSpec spec) {
		NativeWindow* window = nullptr;

		switch (spec)
		{
			case EWindowPlatformSpec::GLFW: window = new GLFWPlatformWindow();
			case EWindowPlatformSpec::SDL: VI_ASSERT("SDL Window not supported");
			case EWindowPlatformSpec::None: VI_ASSERT("Unknown Window detected");
			default: VI_ASSERT("Unknown Window detected");
		}

		return dynamic_cast<NativeWindow*>(window);
	}
}
```



`GLFWPlatformWindow.h`
```cpp
#include Window.h
class GLFWwindow;
class GLFWPlatformWindow : public NativeWindow {
public:
	GLFWPlatformWindow();
	~GLFWPlatformWindow();
	virtual bool Init(const struct ApplicationConfiguration&, EventDispatcher*) override;
	virtual void Shutdown() override;
	virtual void Swapbuffers() override;
	virtual void PollsEvent() override;
	virtual bool ShouldClose() override;
private:
	GLFWwindow* mWindow;
	WindowData mData;
};
```

`GLFWPlatformWindow.cpp`
```cpp
#include GLFWPlatformWindow.h
...
```
