
we're going to allow people to you know extend Implement themselves and inject into the system so 
we'll make a class abstract interface
the method `Submit()` allow you to submit a entry into a Driver or Policy
- We only forward declare Entry in .h file. We do not include `Entry.h` even in `Channel.cpp` because we only forward `Entry`

```cpp
namespace chil::log
{
	struct Entry;
	class IDriver;
	class IPolicy;

	class IChannel
	{
	public:
		virtual ~IChannel() = default;
		virtual void Submit(Entry&) = 0;
		virtual void AttachDriver(std::shared_ptr<IDriver>) = 0;
		virtual void AttachPolicy(std::unique_ptr<IPolicy>) = 0;
	};

	class Channel : public IChannel
	{
	public:
		Channel(std::vector<std::shared_ptr<IDriver>> driverPtrs = {});
		~Channel();
		void Submit(Entry&) override;
		void AttachDriver(std::shared_ptr<IDriver>) override;
		void AttachPolicy(std::unique_ptr<IPolicy>) override;
	private:
		std::vector<std::shared_ptr<IDriver>> driverPtrs_;
		std::vector<std::unique_ptr<IPolicy>> policyPtrs_;
	};
}
```

# relationship
Association: contain a pointer to a Channel.