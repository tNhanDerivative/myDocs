# Source
[ví dụ từ inpyjama trùng với quantumleap](https://inpyjama.com/event-driven/)
# Chú ý ép kiểu function
```cpp
static void BlinkyButton_dispatch(BlinkyButton * const me, Event const * const e)
```


```cpp
void BlinkyButton_ctor(BlinkyButton * const me) {
    Active_ctor(&me->super, (DispatchHandler)&BlinkyButton_dispatch);
```

trong khi DispatchHandler định nghĩa như sau:
```cpp
typedef void (*DispatchHandler)(Active * const me, Event const * const e);
```

Tạm thời kết luận con trỏ hàm thực ra chỉ là void* hoàn toàn an toàn để ép kiểu

# Design
Cần Event struct
```cpp

typedef uint16_t Signal; /* event signal */


enum ReservedSignals {
    INIT_SIG, /* dispatched to AO before entering event-loop */
    USER_SIG  /* first signal available to the users */
};

/* Event base class */
typedef struct {
    Signal sig; /* event signal */
    /* event parameters added in subclasses of Event */
} Event;
```


cần Active Event loop method để tạo thread AO

```cpp
static void Active_eventLoop(void *pvParameters) {
...

    for (;;) {   /* for-ever "superloop" */
		...
        xQueueReceive(me->queue, &e, portMAX_DELAY); /* BLOCKING! */
        configASSERT(e != (Event const *)0);

        /* dispatch event to the active object 'me' */
        (*me->dispatch)(me, e); /* NO BLOCKING! */
    }
}
```

cần Post event method tạo thread để post event

```cpp
void Active_post(Active * const me, Event const * const e) {
BaseType_t status = xQueueSendToBack(me->queue, (void *)&e, (TickType_t)0);

}
```


Client sẽ cung cấp dispatch function để gán cho dispatch pointer
Client tạo một children Class của Active Object thêm vào TimeEvent hay data nào đó
Client tạo constructor để tạo Active Object constructor và khởi tạo các biến còn lại

```cpp
```