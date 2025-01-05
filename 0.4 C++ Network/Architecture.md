
Client và Server sẽ periodically kiểm tra Message_Queue_In
Client và Server có thể gởi Message out bất cứ lúc nào sử dụng connection `m_connection->Send(msg);`

Client và server sẽ sở hữu Message_Queue_In
Connection sẽ sở hữu Message_Que_Out

Connection sẽ nhận Message và deposit vào Message_Queue_In của Client hoặc server


Tại sao Message_Queue phải thread safe: 
vì ở Client, Message_Queue_In có thể được truy bởi cả client và connection, connection sẽ write vào Queue một cách ngẫu nhiên.
Ở Server, nhiều connection từ nhiều client có thể write vào Message_Queue_In ngẫu nhiên.
