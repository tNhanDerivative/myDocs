


## Fedora update package
`sudo dnf update`
## Update software by .rpm file
```bash
sudo rpm -Uvh Path/to-rpm-file/**.rpm
```
```
sudo dnf install Path/to-rpm-file/**.rpm
```

## Find installed package

```
dnf list installed | grep -i nvidia
```





## Flatpak 
Command :	sudo dnf install flatpak
### Add Flathub repo (the best place to get Flatpak app)
```
flatpak --user remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```


