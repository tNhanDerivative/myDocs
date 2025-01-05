Dạng thực thi `Pimpl`  trong đó `impl` không phải là một thành phần chứa trong 1 class lớn mà nó là concrete class của 1 base class.
Để dùng object thì trong base class có 1 method dc define trong base class nhưng được implement trong child class


`Child.h`
```cpp
#pragma once
#include <memory>

class IChild
{
public:
	static std::unique_ptr<IChild> Spawn();
	virtual ~IChild() = default;
	virtual void Terminate() = 0;
};
```


```cpp
#include "Child.h"
#include <boost/process.hpp>


namespace bp = boost::process;

class Child : public IChild
{
public:
	Child()
		:
		child_{ R"(..\x64\Debug\WindowApp.exe)",
			"--run-mode", "GeneratorCoro",
			bp::start_dir(R"(..\x64\Debug)") }
	{}
	void Terminate() override
	{
		child_.terminate();
		child_.wait();
	}
private:
	bp::child child_;
};

std::unique_ptr<IChild> IChild::Spawn()
{
	return std::make_unique<Child>();
}
```

`main.cpp`
```cpp
#include <iostream>
#include "Child.h"
#include <thread>

void Boot()
{
	log::Boot();

	ioc::Get().Register<log::ISeverityLevelPolicy>([] {
		return std::make_shared<log::SeverityLevelPolicy>(log::Level::Info);
	});
}

int main(int argc, const char** argv)
{
	Boot();

	auto pChild = IChild::Spawn();
	std::cout << "Child spawned!\n";
	std::this_thread::sleep_for(2s);
}
```