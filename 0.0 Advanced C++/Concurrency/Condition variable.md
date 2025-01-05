
# Overview
Công dụng của `std::condition_variable`
1. Notify thay đổi của the condition
2. Hibernate 1 hay nhiều thread cho đến khi một thread khác thay đổi the condition và notify thread khác.

# Thread thay đổi condition cần 

	1. Acquire std::mutex điển hình là qua lock_quard()
	2. Thực hiện operation trong đó thay đổi shared variable, conditon
	3. Mở lock mutex rồi dùng std::condition_variable gọi notify_one hoặc notify_all (với trường hợp lock scope thì notify cuối scope ko cần mở lock trước)