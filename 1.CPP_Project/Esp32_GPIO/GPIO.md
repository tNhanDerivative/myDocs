[Embedded tutorial](https://embeddedtutorials.com/eps32/esp32-gpio-in-cpp-part-2/)


## Constructor inheritance

Hai class con đều có một method có mục đích khởi tạo cùng một tập variable nhưng mỗi class một giá trị default khác nhau

```cpp
    constexpr GpioBase(const gpio_num_t pin,
                const gpio_config_t& config, 
                const bool invert_logic = false) :
        _pin{pin},
        _inverted_logic{invert_logic},
        _cfg{config}
    { 

    }
```

`const gpio_config_t& config` this is a const reference so `_cfg{config}` will trigger a copy constructor.

GPIO output và input nên có config default và ta không muốn user tạo ra những struct config này

```cpp
constexpr GpioOutput(const gpio_num_t pin, const bool active_low = false) :
	GpioBase{pin, 
				gpio_config_t{
					.pin_bit_mask   = static_cast<uint64_t>(1) << pin,
					.mode           = GPIO_MODE_OUTPUT,
					.pull_up_en     = GPIO_PULLUP_DISABLE,
					.pull_down_en   = GPIO_PULLDOWN_ENABLE,
					.intr_type      = GPIO_INTR_DISABLE
				}, 
				active_low}
{
	
}
```

