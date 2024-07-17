
# Source 
[Nhàn Hơn Cùng Git Stash! - Viblo](https://viblo.asia/p/nhan-hon-cung-git-stash-07LKXM8JZV4)


Muốn pull code mới. Ta save code đang làm nhưng chưa commit dc

```cpp
git stash
```

# Apply git stash

To apply a stash and remove it from the stash list, run:

```
git stash pop stash@{n}
```

To apply a stash and keep it in the stash cache, run:

```
git stash apply stash@{n}
```

# Show git stash
From `man git-stash` (which can also be obtained via `git help stash`):

> The modifications stashed away by this command can be listed with `git stash list`, inspected with `git stash show`, and ...

```
show [<stash>]
    Show the changes recorded in the stash as a diff between the stashed
    state and its original parent. When no <stash> is given, shows the
    latest one. By default, the command shows the diffstat, but it will
    accept any format known to git diff (e.g., git stash show -p stash@{1}
    to view the second most recent stash in patch form).
```

Note: the `-p` option generates a _patch_, as per `git-diff` documentation.

List the stashes:

```
git stash list
```

Show the _files_ in the most recent stash:

```
git stash show
```

Show the _changes_ of the most recent stash:

```
git stash show -p
```

Show the _changes_ of the named stash:

```
git stash show -p stash@{1}
```

Or in short:

```
git stash show -p 1 
```

If you want to view changes of only the last stash:

```
git stash show -p 0
```