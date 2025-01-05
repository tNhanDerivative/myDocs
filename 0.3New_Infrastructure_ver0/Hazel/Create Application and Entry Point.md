
# EntryPoint
Create application is something you want to implement in the Client Application. Where you want Client app decide how the App object gonna be like
The main function (which own the application object) you want to implement in the Core library how to own the object

EntryPoint.h
```cpp
extern Hazel::Application* Hazel::CreateApplication(ApplicationCommandLineArgs args);

int main(int argc, char** argv)
{

	auto app = Hazel::CreateApplication({ argc, argv });
	app->Run();
	delete app;

}
```

SandBox.cpp
```cpp
#include <Hazel.h>
#include <Hazel/Core/EntryPoint.h>

#include "Sandbox2D.h"
#include "ExampleLayer.h"

class Sandbox : public Hazel::Application
{
public:
	Sandbox(const Hazel::ApplicationSpecification& specification)
		: Hazel::Application(specification)
	{
		// PushLayer(new ExampleLayer());
		PushLayer(new Sandbox2D());
	}

	~Sandbox()
	{
	}
};

Hazel::Application* Hazel::CreateApplication(Hazel::ApplicationCommandLineArgs args)
{
	ApplicationSpecification spec;
	spec.Name = "Sandbox";
	spec.WorkingDirectory = "../Hazelnut";
	spec.CommandLineArgs = args;

	return new Sandbox(spec);
}

```

But finally it will include inside SandBox.cpp which is the execute target, somehow through include.
You don't need to find exactly how. Just need to know it's in the include library