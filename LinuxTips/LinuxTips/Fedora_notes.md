[/toc/] 
{{toc}} 
[[__TOC__]] 
[toc]

## Fedora update package
sudo dnf update

## Change En/Vi typing
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


## Terminal problem
### Change hostname
sudo hostnamectl set-hostname "New_Custom_Name"


### Change .bashrc file to change comand prompt red and bold
PS1="\e[0;31;1m[\u@\h \W]\$ \e[m"

## Coppy paste terminal problem
https://superuser.com/questions/1532688/pasting-required-text-into-terminal-emulator-results-in-200required-text

There is an additional way that this problem can manifest. Readline's `~/.inputrc` file can contain the following:

```
set enable-bracketed-paste On
```
This is not fixed by any of the above methods and has eluded me for months. Removing this line or setting

```
set enable-bracketed-paste Off
```



### Synth Shell

```bash
nano ~/.config/synth-shell/synth-shell-prompt.config
```
font_color_user="white"
background_user="53"
texteffect_user="bold"

font_color_host="white"
background_host="169"
texteffect_host="bold"

font_color_pwd="dark-gray"
background_pwd="227"
texteffect_pwd="bold"

```bash
nano ~/.config/synth-shell/synth-shell-prompt.sh
```

find PS1="$PS1 