

Optimize usage of our resource by managing ownership of smart pointer to static
Avoiding memory fragmentation

## RTOS memory management
When you create a new task that task is assigned a portion of memory from the heap contain TCB (keep information of the Task) and a memory set aside as the Stack of that task
There's a way to allocate static memory for task and kernel object in the newer version of freertos
But esp idf as I know haven't support allocate static memory by default