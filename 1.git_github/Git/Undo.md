git- This will unstage all files you might have staged with `git add`:
    
    ```
      git reset
    ```
    
- This will revert all local uncommitted changes (should be executed in repo root):
    
    ```
      git checkout .
    ```
    
    You can also revert uncommitted changes only to particular file or directory:
    
    ```
      git checkout [some_dir|file.txt]
    ```
    
    Yet another way to revert all uncommitted changes (longer to type, but works from any subdirectory):
    
    ```
      git reset --hard HEAD
    ```
    
- This will remove all local untracked files, so _only_ git tracked files remain:
    
    ```
      git clean -fdx
    ```
    
    > **WARNING:** `-x` will also remove all ignored files, including ones specified by `.gitignore`! You may want to use `-n` for preview of files to be deleted.
    

---

To sum it up: executing commands below is basically equivalent to fresh `git clone` from original source (but it does not re-download anything, so is much faster):

```
git reset
git checkout .
git clean -fdx
```

Typical usage for this would be in build scripts, when you must make sure that your tree is absolutely clean - does not have any modifications or locally created object files or build artefacts, and you want to make it work very fast and to not re-clone whole repository every single time.

# undo git commit --amend

What you need to do is to create a new commit with the same details as the current `HEAD` commit, but with the parent as the previous version of `HEAD`. `git reset --soft` will move the branch pointer so that the next commit happens on top of a different commit from where the current branch head is now.

```bash
# Move the current head so that it's pointing at the old commit
# Leave the index intact for redoing the commit.
# HEAD@{1} gives you "the commit that HEAD pointed at before 
# it was moved to where it currently points at". Note that this is
# different from HEAD~1, which gives you "the commit that is the
# parent node of the commit that HEAD is currently pointing to."
git reset --soft HEAD@{1}

# commit the current tree using the commit details of the previous
# HEAD commit. (Note that HEAD@{1} is pointing somewhere different from the
# previous command. It's now pointing at the erroneously amended commit.)
# The -C option takes the given commit and reuses the log message and
# authorship information.
git commit -C HEAD@{1}
```