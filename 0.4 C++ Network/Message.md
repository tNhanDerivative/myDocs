
# Overview
- Ở compile time, chúng ta có thể không biết dc size của data sẽ dc transmit và received.
=> Dùng raw byte vector

- Nhưng ở runtime chúng ta không thể transmit và read data mà ko biết size.
=> Vậy header với fixed size sẽ dc send "trước" chứa thông tin về payload. Gồm ID và size
## Message header
![[Message_Network.png]]
size có kiểu unint32_t để bảo đảm compile ở cả trên server và thiết bị đều ra 1 kiểu.
id là 1 enum class lý do [[Enum]]
Message này được define trong Core library mà sử dụng cho nhiều dự án khác nhau => ta sẽ ko định nghĩa enum class này mà dùng template để truyền enum ở tầng Application.

```cpp
        template<typename T>
        struct MessageHeader{
            uint32_t size = 0;
            T id = 0;
        }
```

## Message body

Tại vì ta đã templating `MessageHeader` nên mọi thứ depend on `MessageHeader` đều là template

## friend `ostream` function
