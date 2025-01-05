
# Overview

# Application class

[Application class github](https://github.com/quang-pth/GameEngine/blob/ep3/setup-project/Engine/VIEngine/Core/Application.h)
Application.h
```cpp

struct ApplicationConfiguration {
	int Width, Height;
	const char* Title;
};

class Application {
public:
	virtual ~Application() = default;
	virtual bool Init() { return true; }
	void Run();
	virtual void Shutdown() {}
protected:
	Application() = default;
	Application(const ApplicationConfiguration&);
private:
	ApplicationConfiguration mConfig;
};

extern Application* CreateApplication();

```

Ta muốn tạo Client Application (Game, Sandbox) ở client side để quyết định những yếu tố như là Graphic, update nhưng t lại muốn quản lý ở Core các vấn đề start, stop, tạo thread, khởi động lại.
=> `main()` thể hiện thứ tự thứ tự start, stop, tạo thread ... được viết trong `EntryPoint`  rồi include vào Client App file (`Game.cpp`, `Sandbox.cpp`)
=> Để Core library compile `EntryPoint` chỉ cần biết type của con trỏ base (có những method cần thiết mà nó quản lý)
`Application` là base class khai báo trong Core mà Client Application (Game, Sandbox) sẽ thừa kế.

## `CreateApplication()`
`extern Application* CreateApplication();` là một hàm tạo ra Client App => implement ở client side.
Và hàm này trả về 1 con trỏ base tron `EntryPoint` để compile (signature hàm ko cần biết client app)

Vậy là ở trong `SandBox` implement của hàm này rõ ràng sẽ khai báo 1 con trỏ base, tiến hành tạo Sandbox rồi gán giá trị cho con trỏ base.

Đó là lý do chúng ta muốn có 1 hàm create object bên ngoài child class để nó có thể compile ở trong library và run ở client. 

`extern Application* CreateApplication();`  ta để vào trong namespace để không gây nhầm lẫn vì tên khá chung chung.


## Factory method
Ta ko muốn sử dụng `default constructor` để khởi tạo Application mà tác giả muốn ta dùng đúng constructor khác để truyền parameter vào. Do đó ta bỏ default constructor vào trong `protected` hoặc `private`
