
# Overview
Client interface sẽ chịu trách nhiệm set up asio, Connection object 
Đồng thời Client Interface là access point cho Client Application giao tiếp với server

# resource

Mặc dù [[Connection]] sẽ handle asio resource nhưng Client sẽ handle việc kiểm tra condition sau đó set up [[Connection]]
Vậy nên Client sẽ construct 1 unique pointer `Socket`  

	- thread safe message queue: chứa incoming message từ server
	- asio::io_context: handle event 
	- thread: cần có thread riêng để execute command
	- connection: 