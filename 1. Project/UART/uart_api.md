[video](https://www.youtube.com/watch?v=BYxR2EZZvhc&list=PLowIV8ZSSsAXRAsxyKArbY4EEqvUnYNtn&index=2)
[github](https://github.dev/howroyd/embedded_cpp20)

## Static declare
```cpp
static constexpr std::size_t uart_n_hw_ports = static_cast<std::size_t>(UART_ID::UART_INVALID)+1;
static_assert( (uart_n_hw_ports - 1) < static_cast<std::size_t>(UART_ID::UART_INVALID) );
```

`static`: khai báo một giá trị chiếm tồn tại cùng suốt vòng đời của chương trình, ghi nhớ giá trị thay đổi sau mỗi lần gọi
nó khác `global variable` ở chỗ nó có thể là member của một class private, protected
[static lý thuyết](https://techacademy.edu.vn/static-trong-c/)

`constexpr`: keyword in C++ is used to declare a variable or function that can be evaluated at compile-time
[lý thuyết constexpr](https://codecungnhau.com/hieu-ve-constexpr-trong-c/)

`static_cast`: operator in C++ is used for explicit type conversions. It performs compile-time type checking

`std::size_t>`: nó là typedef của unsigned int

`static_assert`: are a way to check if a condition is true when the code is compiled. If it isn't, the compiler is required to issue an error.

## api_uart_send()
```
std::this_thread::sleep_for(std::chrono::seconds(2));
```
The `std::chrono` is a namespace in the C++ Standard Library that provides user-defined literals for time durations such as hours, minutes, seconds, milliseconds, microseconds, and nanoseconds

```cpp
using namespace std::chrono_literals;

std::this_thread::sleep_for(1ms);
```




## api_uart_receive

```cpp
int numbers[10]; 
std::iota(numbers, numbers + 10, 100);
```

In this example, `std::iota` fills the array `numbers` with 10 values starting from 100 and incrementing by 1