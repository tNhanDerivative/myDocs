
# CMSIS file structure
[ CMSIS Documentation ](https://www.keil.com/pack/doc/cmsis/Core/html/templates_pg.html)
System file: Setting up the clock register

Device file: Define address of peripherals. 
Ví dụ: Device file chứa struct UART0_Type là cấu trúc thanh ghi của UART0; có UART0_BASE là địa chỉ của thanh ghi UART0
```c
#define UART0        ((UART0_Type *)    UART0_BASE)
```
Ta có thể dùng thanh ghi CTL của UART0 như sau
```c
UART0->CTL
```
# Download CMSIS
[Download TI CMSIS ](https://www.ti.com/tool/CMSIS_DSP_HEADERS)