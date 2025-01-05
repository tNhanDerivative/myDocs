# Overview

![https://denyskryvytskyi.github.io/assets/img/event-system/class-diagram.png](https://denyskryvytskyi.github.io/assets/img/event-system/class-diagram.png)



# EventAction
how to ask [[ChatGPT]]

Idea: `mCallback` là function thực hiện hành động mà ta cần
ta thiết kế `mCallback` là một function template nhận parameter type T ( child class của `EventContext`)

1. `IEventAction`: An interface class for event actions with a virtual Execute method.
	- Provides a common base for all event actions
	- Allows for polymorphic behavior through the virtual Execute method
	
2. `EventCallback` template function:  that takes a const reference `EventContext` and returns a bool.
	- Allows for type-safe callbacks specific to each event type
	- Uses std::function for flexibility in callback types (e.g., lambdas, member functions)

3. `EventAction`template class: derived from `IEventAction`, implementing the Execute method for specific event types.
	- Implements `IEventAction` for specific event types
	- Stores the callback of the correct type
	- Execute method casts the generic `EventContext` to the specific type `EventCallback<T> mCallback` and calls it
	
# Dispatcher
`const char* eventType = typeid(T).name()`
ta dùng tên của type T ( child class của `EventContext`) để làm key value trong `mEventActionMap`

`EventDispatcher` need to keep track of which Observers are interested in which Event
We need a predefined `EventAction`interface that  every Observer needs to implement

`EventActionList`: An alias for std::vector<IEventAction*>, representing a list of event actions.
`EventDispatcher`: The main class managing event registration and dispatching.

# Event Action

`EventAction` is Event callback wrapper  will use template to be able to store any type of event handler with the appropriate event type reference (instead of unsafe void pointer to Event):

# To do
Làm sao để tìm ra type T là child class của `EventContext`
```cpp
VI_STATIC_ASSERT(std::is_base_of<EventContext, T>::value && "Invalid event context");
```

Static assertion in Execute:

- Ensures at compile-time that T is derived from EventContext
- Prevents misuse with incompatible types