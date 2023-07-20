### git log graph

```bash
git log --all --decorate --oneline --graph
```

### …or create a new repository on the command line

```bash
echo "# nhan_TheNoobMaster" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/tNhanDerivative/nhan_TheNoobMaster.git
git push -u origin main
```

### …or push an existing repository from the command line

```bash
git remote add origin https://github.com/tNhanDerivative/nhan_TheNoobMaster.git
git branch -M main
git push -u origin main
```

### Uncommit last commit
```
git reset --soft HEAD~1
```

### change branch name
`git branch -m master main`

## Create new branch
Before creating a new branch, pull the changes from upstream. Your master needs to be up to date.

`$ git pull`

Create the branch on your local machine and switch in this branch :

`$ git checkout -b [name_of_your_new_branch]`

Push the branch on github :

`$ git push origin [name_of_your_new_branch]`

When you want to commit something in your branch, be sure to be in your branch. Add -u parameter to set-upstream.
You can see all the branches created by using :

`$ git branch -a`