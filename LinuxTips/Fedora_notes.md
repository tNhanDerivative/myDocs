


## Fedora update package
`sudo dnf update`

## Find installed package

```
dnf list installed | grep -i nvidia
```
## Update software by .rpm file
```bash
sudo rpm -Uvh Path/to-rpm-file/**.rpm
```

## Change En/Vi typing (Gnome)
```
Super + Space 							
```

## Split Screen
```
Super + Arrow_key 							
```


## Flatpak 
Command :	sudo dnf install flatpak
### Add Flathub repo (the best place to get Flatpak app)
```
flatpak --user remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```


