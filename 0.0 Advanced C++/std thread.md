# Why join or detach
Như vậy thread sẽ dc terminate đúng cách khi parent thread terminated.
Nếu ko join hay detach, thread sẽ rơi vào trạng thái zombie sau khi thực thi xong
If the main thread finishes before a detached thread completes, the detached thread will be terminated corectly
Double `join()` or `detach()`: Attempting to join or detach a thread that has already been joined or detached will result in a runtime error. Always check with `joinable()` before joining or detaching.

### 2.1.4. detach( ) Running threads in the background
[Source ](https://livebook.manning.com/book/c-plus-plus-concurrency-in-action/chapter-2/42)

Detached threads truly run in the background; ownership and control are passed over to the C++ Runtime Library, which ensures that the resources associated with the thread are correctly reclaimed when the thread exits.

Detached threads thực sự chạy dưới background; ownership và control được truyền sang Thư viện Runtime C++, đảm bảo rằng các resource liên quan đến thread được thu hồi khi thread exits..

Các thread như vậy thường chạy lâu; chúng có thể chạy gần như toàn bộ vòng đời của ứng dụng, thực hiện một tác vụ nền như theo dõi hệ thống tệp, xóa các mục không được sử dụng khỏi object cache hoặc tối ưu hóa cấu trúc data. 

Ở một khía cạnh khác, có thể có ý nghĩa khi sử dụng một Detached threads khi có một cơ chế khác để xác định khi nào luồng đã hoàn thành hoặc khi luồng được sử dụng cho một tác vụ "fire and forget".

Sau khi `detach()` thread ko thể join được nữa.

###  Ví dụ

Hãy xem xét một ứng dụng như trình xử lý văn bản có thể chỉnh sửa nhiều tài liệu cùng một lúc. Có nhiều cách để xử lý điều này, cả ở cấp độ UI và internal.  Một cách  phổ biến là có nhiều window độc lập, mỗi window cho một tài liệu đang được chỉnh sửa.

Although these windows appear to be completely independent, each with its own menus and so forth, they’re running within the same instance of the application. One way to handle this internally is to run each document-editing window in its own thread; each thread runs the same code but with different data relating to the document being edited and the corresponding window properties. 

Opening a new document therefore requires starting a new thread. The thread handling the request isn’t going to care about waiting for that other thread to finish, because it’s working on an unrelated document, so this makes it a prime candidate for running a detached thread

```cpp
void edit_document(std::string const& filename) {
    open_document_and_display_gui(filename);

    while (!done_editing()) {
        user_command cmd = get_user_input();

        if (cmd.type == open_new_document) {
            std::string const new_name = get_filename_from_user();
            std::thread t(edit_document, new_name); // 1
            t.detach(); // 2
        } else {
            process_user_input(cmd);
        }
    }
}
```
If the user chooses to open a new document, you prompt them for the document to open, start a new thread to open that document ![](https://drek4537l1klr.cloudfront.net/williams/Figures/1.jpg), and then detach it ![](https://drek4537l1klr.cloudfront.net/williams/Figures/2.jpg). Because the new thread is doing the same operation as the current thread but on a different file, you can reuse the same function (edit_document) with the newly chosen filename as the supplied argument.

This example also shows a case where it’s helpful to pass arguments to the function used to start a thread: rather than just passing the name of the function to the std::thread constructor ![](https://drek4537l1klr.cloudfront.net/williams/Figures/1.jpg), you also pass in the filename parameter. Although other mechanisms could be used to do this, such as using a function object with member data instead of an ordinary function with parameters, the Thread Library provides you with an easy way of doing it.