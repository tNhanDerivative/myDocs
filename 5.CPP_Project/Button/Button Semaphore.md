
# Button and Semaphore

```cpp
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "freertos/semphr.h"
#include "driver/gpio.h"

#define BUTTON_GPIO 35
#define LED_GPIO 5

SemaphoreHandle_t xSemaphore = NULL;

void IRAM_ATTR button_isr_handler(void* arg) {
    BaseType_t xHigherPriorityTaskWoken = pdFALSE;
    xSemaphoreGiveFromISR(xSemaphore, &xHigherPriorityTaskWoken);
    if (xHigherPriorityTaskWoken) {
        portYIELD_FROM_ISR();
    }
}

void lighting_task(void* pvParameters) {
    while (1) {
        if (xSemaphoreTake(xSemaphore, portMAX_DELAY)) {
            gpio_set_level(LED_GPIO, 1); // Turn on LED
            vTaskDelay(1000 / portTICK_PERIOD_MS); // Wait for 1 second
            gpio_set_level(LED_GPIO, 0); // Turn off LED
        }
    }
}

void app_main() {
    gpio_config_t io_conf;
    io_conf.intr_type = GPIO_INTR_POSEDGE; // Interrupt on the rising edge
    io_conf.mode = GPIO_MODE_INPUT;
    io_conf.pin_bit_mask = 1ULL << BUTTON_GPIO;
    io_conf.pull_down_en = 0;
    io_conf.pull_up_en = 1; // Enable pull-up resistor
    gpio_config(&io_conf);

    // Configure the ISR
    gpio_install_isr_service(0);
    gpio_isr_handler_add(BUTTON_GPIO, button_isr_handler, NULL);

    // Configure the LED
    gpio_config_t io_conf_led;
    io_conf_led.intr_type = GPIO_INTR_DISABLE;
    io_conf_led.mode = GPIO_MODE_OUTPUT;
    io_conf_led.pin_bit_mask = 1ULL << LED_GPIO;
    io_conf_led.pull_down_en = 0;
    io_conf_led.pull_up_en = 0;
    gpio_config(&io_conf_led);

    // Create the semaphore
    xSemaphore = xSemaphoreCreateBinary();

    // Create the Lighting task
    xTaskCreate(lighting_task, "Lighting", 2048, NULL, 5, NULL);
}

```