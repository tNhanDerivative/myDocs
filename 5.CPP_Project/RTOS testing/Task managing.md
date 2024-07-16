

# Template
Generic bằng void* pointer và template
truyền vào void* pointer và ép kiểu bằng template

```cpp
template <typename T_task>
void rtos_task(void *task_object) {
    ( *static_cast<T_task *>(task_object) )();
}
```

Như vậy `rtos_task` sẽ giữ nguyên type của  `TaskFunction_t`. 

```cpp
typedef void (*TaskFunction_t)( void * );

BaseType_t xTaskCreate(TaskFunction_t pvTaskCode,
						const char *pcName,
						const uint16_t usStackDepth,
						void *pvParameters,
						UBaseType_t uxPriority,
						TaskHandle_t *const pxCreatedTask);
```

Nhờ template mà ta ko cần viết lại wrapper `rtos_task` tham khảo phương pháp cũ của C để ép kiểu tức là phải viết lại wrapper cho từng task_function
```cpp
typedef struct {
    int id;
    float value;
} MyTaskParams;

void myTask(void *pvParameter) {
    MyTaskParams *params = (MyTaskParams *)pvParameter;
}
```


# pass in specific type for template
You need to explicitly pass in the type to a template in situations where the compiler cannot deduce the template argument from the function arguments.

```cpp
template <typename T>
void process(T value) {
    // Implementation
}

// Calling the function with an integer
process(42); // Compiler can deduce T as int

// Calling the function with a float
process(3.14f); // Compiler can deduce T as float

// Ambiguous call - the compiler cannot deduce T
process(42.0f); // Must explicitly specify T as float
process<float>(42.0f); // Correct way to specify the type explicitly
```

In the last call, the argument `42.0f` could be interpreted as either an `int` or a `float`, so the compiler requires explicit specification to resolve the ambiguity.



Nhờ tem