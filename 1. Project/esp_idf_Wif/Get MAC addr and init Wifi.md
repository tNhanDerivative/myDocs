
Wifi.h
```cpp
class Wifi{

public:
constexpr static const char* get_mac(void) { return mac_addr_cstr; }

private:
    // Get the MAC from the API and convert to ASCII HEX
    static esp_err_t _get_mac(void);

    static char mac_addr_cstr[13];  ///< Buffer to hold MAC as cstring
    static std::mutex init_mutx;    ///< Initialisation mutex
}

```


Wifi.cpp
```cpp
// Wifi statics
char                Wifi::mac_addr_cstr[]{};    ///< Buffer to hold MAC as cstring
std::mutex          Wifi::init_mutx{};          ///< Initialisation mutex

Wifi::Wifi(void)
{
    // Aquire our initialisation mutex to ensure only one
    //   thread (multi-cpu safe) is running this
    //   constructor at once.  No running twice in parallel!
    std::lock_guard<std::mutex> guard(init_mutx);

    // Check if the MAC cstring currently begins with a
    //   nullptr, i.e. is default initialised, not set
    if (!get_mac()[0])
    {
        // Get the MAC and if this fails restart
        if (ESP_OK != _get_mac())
            esp_restart();
    }
}
```
`mac_addr_cstr[13]` là member của class được khai báo là static
`_get_mac()`: sẽ thực sự lấy MAC addr từ config của esp32 và gán vào biến `mac_addr_cstr[13]`
`get_mac()`: chỉ return giá trị biến `mac_addr_cstr`

Lúc khởi tạo Wifi đầu tiên ta chỉ gọi hàm `_get_mac` 1 lần đầu và sau đó nếu có 1 thread khác khởi tạo Wifi thì hàm constructor ở trên sẽ kiểm tra biến `mac_addr_cstr` có phải null ko bằng hàm `get_mac`.

Mutex là để bảo vệ đoạn code khỏi race condition



## static
[[Static]]
- Khi chúng ta khai báo 1 thành viên của 1 lớp là static, lúc đó dù cho có bao nhiêu đối tượng của lớp được tạo, thì sẽ chỉ mang 1 bản sao của thành viên static
- Tuy nhiên chúng ta không thể gán trước giá trị ban đầu của static trong class mà chúng ta phải khởi tạo giá trị cho nó ở ngoài class
- Hàm và biến khai báo static độc lập với bất kỳ đối tượng cụ thể nào của lớp. Một hàm thành viên static có thể được gọi ngay cả lúc không có đối tượng nào của lớp tồn tại và những hàm static được truy cập chỉ bởi dùng tên lớp và toán tử phân giải phạm vi ` :: `
- Khi biến static ở private, nên bên ngoài class không thể dùng phạm vi truy xuất đến truy cập (Như cách gọi `Wifi::mac_addr_cstr`) Nhưng việc khai báo và gán giá trị ban đầu là được phép.




