git### Kiểm tra sự thay đổi thư mục làm việc

Khi có sự thay đổi của thư mục làm việc mà chưa chỉ mục, thì có thể xem sự thay đổi của nó với commit cuối
```cpp
git diff
```


### Kiểm tra sự thay đổi của index (staging) với commit cuối
```cpp
git diff --staged
```


### Kiểm tra thay đổi giữa hai commit
```cpp
git diff hash-commit1 hash-commit2
```

### Kiểm tra sự thay đổi của hai nhánh
```cpp
git diff branch1 branch2
```

For instance, to see the difference for a file "main.c" between now and two commits back, here are three equivalent commands:

```
$ git diff HEAD^^ HEAD main.c
$ git diff HEAD^^..HEAD -- main.c
$ git diff HEAD~2 HEAD -- main.c
```
