
## Change hostname
sudo hostnamectl set-hostname "New_Custom_Name"

## get git branch
```bash
parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
PS1="\u@\h \w\n \$(parse_git_branch) \$"
```

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

## Change .bashrc prompt colors
reset the prompt color
`\[\e[0m\]`
Background:
- **40**: Black background
- **41**: Red background
- **42**: Green background
- **43**: Yellow background
- **44**: Blue background
- **45**: Purple background
- **46**: Cyan background
- **47**: White background
Bold, underline, normal text:
- **0**: Normal text
- **1**: Bold or light, depending on terminal
- **4**: Underline text
Colors:
- **30**: Black
- **31**: Red
- **32**: Green
- **33**: Yellow
- **34**: Blue
- **35**: Purple
- **36**: Cyan
- **37**: White
Specify background colors, attributes, and text color all in one go
`\[\e[42;1;36m\]`
separate background information from the other information:
`\[\e[42m\]\[\e[1;36m\]`





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