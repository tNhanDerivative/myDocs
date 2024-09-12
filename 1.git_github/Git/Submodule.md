


# Problem
Khi dùng `git add .` ta dễ add thay đổi mới (new commit) của submodule ko phải do ta làm . Phải sửa ngay vì ta ko biết trong submodule ấy thay đổi gì.

Solution check `git diff` để lấy  commit gốc trong submodule
cd đến submodule checkout đến commit gốc đó
cd trở lại parent project, `git add .` để add thay đổi của submodule.
commit lại.

#  workflow

- commit in submodule 
- push submodule to it's remote repository. 

- `cd` to parent project add new commit of submodule to your commit. Don't worry.
- push parent project to its remote repo

# Khi commit trong submodule

>Trường hợp bạn code một feature có thay đổi trong submodule. Và bạn cần phải thể hiện cái thay đổi đó đi cùng với nhau trong một commit
Trước tiên bạn cần cd vào submodule push và merge code mới vào nhánh chung của submodule đó. (Bao gồm fix code và rebase đầy đủ)
Sau đó bạn pull merge đó về local của submodule.

Lúc này cd qua nhánh chính, bạn sẽ thấy
```
 modified:   /submodule/path (new commits)
```
bây h bạn `git add /submodule/path` (việc này sẽ update tên commit lưu trong main repo) và push cùng code thay đổi trong main repo
```sh
cd path/to/parent/repo
# Update the submodule reference
git add path/to/submodule
git commit -m "Update submodule to latest commit"
git push origin master
```

# giải thích
Khi bạn 
```sh
git submodule update
```
bạn chỉ update tên `submodule commit` theo upstream repo. Do đó nếu bạn `git submodule update` repo thì commit sẽ chỉ update theo `uptream` của main repo



## 1. Commit in Third-Party Submodule

First, navigate to the submodule directory and perform your changes. Then, commit those changes within the submodule.
```sh
cd path/to/submodule
# Make your changes here
git add .
git commit -m "Your commit message"

```

```sh
git push origin master
```
Sau khi dc approve => Merge pull request.

Pull merge mới về
```sh
git checkout [release_name]
git pull origin [release_name]
```

## 2. Add New Submodule Pointer from Parent Repo to New Commit


