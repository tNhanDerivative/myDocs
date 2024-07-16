# tutorial
[What? Why? When? Tutorial Youtube](https://www.youtube.com/watch?v=gSlXo2iLBro&t=295s)
# Update submodule from Main module
when you type `git status` and the out put are like this
```sh
$ git status
On branch MDE-922
Your branch is ahead of 'origin/release-radhanagar2' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   owc-ui (new commits)
        modified:   shared/cross-platform (new commits)
        modified:   xplat/3rdparty (new commits)
```

submodule: `modified:   owc-ui (new commits)` indicate that this submodule has a new commits
```sh
git submodule update --recursive
```
```sh
git submodule update --init --recursive
```

--recursive option will make it update recursively
--init option which will make it initialize any uninitialized submodules.

Read more how to create here[[Create submodule]]

# Update submodule from Submodule

Suppose you have a submodule named `my-submodule` in your main project, and you want to update it to the latest commit on its `master` branch. Here's how you would do it:

```sh
# Navigate to the submodule directory
cd libs/my-submodule

# Checkout the master branch
git checkout master

# Pull the latest changes
git pull

# Return to the main project root
cd../../..

# Stage and commit the submodule update
git add libs/my-submodule
git commit -m "Updated my-submodule to latest commit"

# Push the changes
git push
```