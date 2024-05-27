

# install Ruby
```bash
sudo dnf install ruby
```
# install gcc
```bash
gem install ceedling
```

# Check

```bash
ceedling
```

 success message look like this
```
Welcome to Ceedling!
Commands:
  ceedling example PROJ_NAME [DEST] # new specified example project (in DEST...
  ceedling examples # list available example projects
  ceedling help [COMMAND] # Describe available commands or one spe...
  ceedling new PROJECT_NAME # create a new ceedling project
``` 

# fix lỗi

```
 undefined method `exists?' for File:Class (NoMethodError)

  project_found = File.exists?(main_filepath)
                      ^^^^^^^^
Did you mean?  exist?
	from /home/trongnhan/bin/ceedling:25:in `load'
	from /home/trongnhan/bin/ceedling:25:in `<main>'
```
vào đường dẫn file đó sửa lại tên function `exists` thành  `exist`


## Lỗi certificate thì xem ở đây
[install ceedling for linux ](https://static1.squarespace.com/static/549f45d6e4b037c1971053fd/t/5a5915639140b75f8713fa0e/1515787619259/installation-guide-linux.pdf)