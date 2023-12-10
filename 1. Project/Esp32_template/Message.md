
# Basic design

Basic Queue is created in the Heap storage

As long as we use built-in kernel function writing to a queue is atomic (which means another task can't interrupt).
Another task read from the Queue asynchornously will automatically free up that item of the queue cause other item to shift forward.
We will set a time-out in the receiving task to wait a specified number of tick if the queue is `empty()` and then return a status code.

you shoudn't use basic queue function to send or receive item from the queue because Interrupt does not depend on tick timer and shouldn't wait any amount of time.


```cpp

    const size_t        queue_len{0};       ///< Number of items the queue can hold
    const size_t        item_n_bytes{0};    ///< Number of bytes per item in the queue

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
	
	
 
```