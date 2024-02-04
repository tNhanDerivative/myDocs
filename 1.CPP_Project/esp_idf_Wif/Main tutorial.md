
# main  source
[main tutorial](https://embeddedexplorer.com/esp32-wifi-station/#Binding_WiFi_driver_and_esp-netif)

[how to create static library](https://www.youtube.com/watch?v=CdmJ9xAYHno)

![](Wifi_programing_model.png )

## Initialize TCP/IP stack

```cpp
esp_err_t status{ESP_OK};
status = esp_netif_init();
```

Khi hit `CTRL + click` vào `esp_netif_init()` ta thấy kiểu dữ liệu trả về là kiểu struct `esp_err_t`
*         - ESP_OK on success  
*         - ESP_FAIL if initializing failed

## Error Hanling
a logging for error
[ESP_ERROR_CHECK Macro](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/error-handling.html#esp-error-check-macro)
