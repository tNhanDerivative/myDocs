
# Explain

Có 1 Event loop nhận event từ Queue `xQueueReceive` và xử lý event.
Khi viết thư viện ta tách ra có 1 function custom xử lý event client là Dispatch
```cpp
#include "FreeRTOS.h"
#include "queue.h"
#include "task.h"

// Define the event structure
typedef struct {
    int type;
    int data;
} Event;

// Queue handle for the Active Object
QueueHandle_t activeObjectQueue;

// Active Object task
void ActiveObjectTask(void *pvParameters) {
    Event event;
    while (1) {
        if (xQueueReceive(activeObjectQueue, &event, portMAX_DELAY)) {
            // Process the event
            switch (event.type) {
                case EVENT_TYPE_1:
                    // Handle event type 1
                    break;
                case EVENT_TYPE_2:
                    // Handle event type 2
                    break;
                // Add more cases as needed
            }
        }
    }
}

// Function to post an event to the Active Object
void PostEvent(int type, int data) {
    Event event = {type, data};
    xQueueSend(activeObjectQueue, &event, portMAX_DELAY);
}

// Main function
int main(void) {
    // Create the queue for the Active Object
    activeObjectQueue = xQueueCreate(10, sizeof(Event));
    if (activeObjectQueue == NULL) {
        // Handle error
    }

    // Start the Active Object task
    xTaskCreate(ActiveObjectTask, "ActiveObjectTask", configMINIMAL_STACK_SIZE, NULL, tskIDLE_PRIORITY + 1, NULL);

    // Start the scheduler
    vTaskStartScheduler();

    // Should never reach here
    for (;;);
}

```