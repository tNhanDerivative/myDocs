# Tạo nhánh theo ticket

>Before creating a new branch, PULL the changes from upstream.

Tạo branch và di chuyển đến branch đó
Check trước khi add và commit
```sh
git status
git diff [ file_path ]
#gitK is better
```

commit vào branch vừa tạo
```sh
git add .
git commit -m "{ Ticket_id } | {Ticket_subject_name} "
```

# Tạo Pull Request
> First of all should Pull code release branch then rebase
Push commit lên nhánh release-2 rồi ask for review
```sh
 git push origin [Ticket_id]
```

Truy cập link remote trong ouput để Tạo Pull Request (PR)

Để xem review => fix code => check và commit lại bằng `git commit --amend`
Lệnh này sửa chữa commit trước đó và gộp lại vào commit trước đó

```sh
git commit --amend
## Thay đổi message theo ý muốn (nhấn insert để bắt đầu nhập thay đổi)
## Lưu lại và thoát (Nhấn Esc + nhập :wq ↲)
```

_Chúng ta cần push force mới áp dụng được thay đổi này lên remote repository_
```sh
git push -f origin [ branch_name(Ticket_id) ]
```

# After approved PR
Sau khi dc approve => Rebase [[Rebase]]

![[git_rebase.png]]
Merge pull request.

Pull merge mới về
```sh
git checkout [release_name]
git pull origin [release_name]
```




OPTIONAL: This command will delete the branch only if it has been fully merged with the branch you are currently on.
```sh
git branch -d branch_name
```

