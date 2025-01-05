# Overview
Connection là class 
	- quản lý các  asio library resource: socket, io_context
	- Sở hữu Message_queue_Out:
	- Mượn Message_queue_In từ Client hoặc server
share io_context với Client hoặc server
Server có nhiều connection share chung 1 context. io_context sẽ được truyền reference từ Client hoặc server. 

```cpp
asio::io_context& m_asioContext;
```

Co
