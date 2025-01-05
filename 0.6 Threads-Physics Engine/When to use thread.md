
# Predictable large volume of small task
Ví dụ như dùng 1 thread riêng để render pixel, tốc độ rất chậm.
nhưng pixel thì rất dễ dàng chia ra để vẽ
[[Project # Multi-thread drawing]]
# CPU intensive blocking task
Ta phải thực hiện một very CPU intensive blocking task trong khi current thread sẽ phải thực hiện các random interrupt task
[[Project # Saving data]]

# IO waiting task
Write to a disk, send a request, write a log