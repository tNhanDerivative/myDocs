
# Basic design
Basic Queue is created in the Heap storage

As long as we use built-in kernel function writing to a queue is atomic (which means another task can't interrupt).
Another task read from the Queue asynchronously will automatically free up that item of the queue cause other item to shift forward.
We will set a time-out in the receiving task to wait a specified number of tick if the queue is `empty()` and then return a status code.

you shouldn't use basic queue function to send or receive item from the  queue because Interrupt does not depend on tick timer and shouldn't wait any amount of time.

```cpp

public:
    const size_t        queue_len{0};       ///< Number of items the queue can hold
    const size_t        item_n_bytes{0};    ///< Number of bytes per item in the queue

    QueueInterface(void) = delete;      ///< Not default constructable

	esp_err_t send()
	esp_err_t receive()
	esp_err_t receive() // move up priority
	esp_err_t peek()
	
	esp_err_t receive()
	size_t n_items_waiting(void)
	size_t n_free_spaces(void)
	
	bool empty(void)
	bool full(void)
	bool clear(void)

protected:
    QueueInterface(std::unique_ptr<QueueDefinition> h_queue,
                    const size_t n_items,
                    const size_t item_n_bytes) :
        queue_len{n_items},
        item_size_bytes{item_n_bytes},
        h_queue{h_queue}
    {
        assert(0 < n_items); assert(0 < item_n_bytes); // TODO review this
    }

    QueueInterface(const QueueInterface& other) :
        queue_len{other.n_items},
        item_size_bytes{other.item_n_bytes},
        h_queue{other.h_queue}
    {}
    
	std::shared_ptr<QueueDefinition> h_queue{}; ///< Shared pointer containing the FreeRTOS queue
    mutable std::recursive_mutex        _send_mutex{};      
    mutable std::recursive_mutex        _receive_mutex{};   
	
 
```

# Interface Qmessage class
## Constructor
We're not going to create the queue in this class so therefore it must be passed into the class.
It make sense to be the unique pointer so that guarantees there's only this class own this handle passed into this class
We're going to create a shared_ptr from unique_ptr

## Copy constructor
 Will create a thread safe copy referring to the same underlying queue
 one copy for the sender and one for the receiver

## Default constructor
if somewhere in our code we call without any parameter
```cpp
QueueInterface SomeQueue{};
```
we will have a big problem.
So ... we should delete the default constructor
```cpp
 QueueInterface(void) = delete;      
```
