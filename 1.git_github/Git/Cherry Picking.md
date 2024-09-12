

# Command

# Use case

Khi mà chúng ta lỡ commit nhầm branch
![[Use_case_cherry_picking.png]]
Việc cần làm là log ra lấy cái commit hash của commit mà ta cần
chuyển sang branch mà chúng ta muốn move commit tới  cherry pick commit ta cần dùng hashq

```sh
git checkout feature_name
git cherry-pick [hash_name]
```

Check out main branch và `git reset` xóa commit thừa đi.
[[Reset, Checkout]]
