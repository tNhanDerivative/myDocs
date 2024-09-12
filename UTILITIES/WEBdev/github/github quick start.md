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