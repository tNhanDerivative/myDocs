[my code](https://github.com/tNhanDerivative/embedded_Class)


[Code talk Embedded pattern to impress your dev friend](https://github.dev/sgbush/cppcon2022/tree/release/code/sections/1_gpio_configuration)

[stackoverflow memory mapped class](https://stackoverflow.com/questions/68687067/memory-mapped-c-objects-non-hardware-members)

[ Dan Saks article](https://www.embedded.com/memory-mapped-devices-as-c-classes/)

[Dan Saks powerpoint](https://cdn2-ecros.pl/event/codedive/files/presentations/2015/Representing-Memory-Mapped-Devices-Saks.pdf)


# Strongly typed input config
Tại sao cần strong type: Có 2 cách config 
1 là dùng mỗi function để config một Register. Nhưng như vậy sẽ rất tốn công config
2 là dùng struct để config 1 lần, nhưng như vậy dễ lỗi  
vì khi lộn thứ tự của PinNumber và mode compiler sẽ ko báo lỗi vì có cùng kiểu dữ liệu (type: uint32_t)
```cpp
using GPIO_Config = \
struct GPIO_Config {
	uint32_t PinNumber;
	uint32_t mode;
	...
}
```

Sử dụng enum để strong type.

```cpp
using gpio_mode_t = enum GPIO_MODE:uint32_t{ INPUT=0, OUTPUT=1, ALT=2}

using GPIO_Config = \
struct GPIO_Config {
	uint32_t PinNumber;
	gpio_mode_t mode;
	...
}
```



