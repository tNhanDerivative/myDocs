
[bài giảng](https://www.youtube.com/watch?v=F6Ipn7gCOsY&list=TLPQMTQxMjIwMjNYZU9JYSrO6Q&index=2)

## Protection must be complete
If you don't protect the read action, another write thread my jump in the midle of reading period and cause data race.

## Exception safety
When Exception throw, it's abort the current function execution and get out of current scope, so we've never execute the line that would have unlock the mutex.
![](exception_safety.PNG )
So we have to look for a way to follow RALL. Mutex is a resource and when it out of scope, the destructor should take care of it.

## lock_guard

the class template `std::lock_guard<T>`  store a reference to the mutex.
lock_guard constructor lock the given mutex.
lock_guard destructor unlock the given mutex.
Consider lock_guard is a RALL wrapper of mutex.

## unique_lock
std::unique_lock manage unique ownership of mutex lock equivalent of std::unique_ptr manage pointer ownership.