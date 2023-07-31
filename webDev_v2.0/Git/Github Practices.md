
## Private branch
Nhánh chỉ có 1 người làm việc. Trên nhánh này mỗi khi commit xong có thể push luôn không băn khoăn ko  conflict

## Situation 1: production branch and local machine branch

### set up for new machine

add github ssh-key for production machine [[SSH]]

clone code về máy => tạo branch máy đó `git branch -b [branch_name]`

### git rebase
https://backlog.com/git-tutorial/vn/stepup/stepup2_8.html 
1. Fetch the latest changes from             `(main)`: `git fetch origin main`
2. Check out your feature branch:           `(main)`:`git checkout my-feature`
3. Rebase                                                   `(my-feature)`: `git rebase origin/main`
4. Force push to your branch.                    `(my-feature)`: `git push -f origin my-feature`
### git rebase conflict
Sau khi resolve conflict
```bash
git add conflictFile.txt
git rebase --continue
```





