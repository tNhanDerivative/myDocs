## Thiết kế
Uart iface là để pass uart_impl vào hw_iface
```cpp
    using hw_iface_t = HardwarePeripheralWrapper<Impl, uart_n_hw_ports>;
    using hw_handle_t = hw_iface_t::handle_t;
    hw_handle_t h{nullptr};
```
uart iface sử dụng uart_impl thông qua handle h của hw_iface 

##
```cpp
template <class T>
bool send(std::span<const T> data)
{
	return h->send(std::forward<std::span<const T>>(data));
}
```
Hàm send này 
The function `h->send(std::forward<std::span<const T>>(data));` is a call to a member function `send` on an object `h`, passing in the forwarded span.

The `std::forward` function is used to forward an lvalue or rvalue, depending on the original argument category. This is known as "perfect forwarding" and is often used in template programming to preserve the value category of forwarded arguments

The `std::span` template parameter `T` is a placeholder for any type, which means the `send` function can handle any type of data

The `std::span<const T>` part indicates that the span is a read-only view of the data, meaning the function will not modify the data it refers to.

The line `template <class T>` is a template declaration in C++.