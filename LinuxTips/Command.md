## update and shut down
```
sudo dnf update && shutdown -P now
```




## Process
<https://www.digitalocean.com/community/tutorials/process-management-in-linux#process-states-in-linux>


## tree
```bash
tree -I "static|virtualenv|static_root" -P "*.py|*.html"
```
`-I` excludes the files in the pattern here separated by a vertical bar. 
`-P` switch inlcudes only the files listed in the pattern matching a certain extension.


## Enviroment Variable
`export` sets the environment variable on the left side of the assignment to the value on the right side of the assignment;

such environment variable is visible to the process that sets it and to all the subprocesses spawned in the same environment,
in this case to the Bash instance that sources `~/.bash_profile` and to all the subprocesses spawned in the same environment (which may include e.g. also other shells, which will in turn be able to access it)

## Find
https://www.linode.com/docs/guides/find-files-in-linux-using-the-command-line/

|Command                                        |Description|
|-----------------------------------------------|--------------------------------------------------------------|
|`find . -name testfile.txt`                       |Find a file called testfile.txt in current and sub-directories.|
|`find /home -name *.jpg`                          |Find all `.jpg` files in the `/home` and sub-directories.|
|`find . -type f -empty`                           |Find an empty file within the current directory.|
|`find /home -user exampleuser -mtime -7 -iname ".db"` |Find all `.db` files (ignoring text case) modified in the last 7 days by a user named exampleuser.|

Option

|Command|Description|
|------------------------|----------------------------------------------------|
|`-not`                           |Return only results that do not match the test case.|
|`-type f`                      |Search for files.|
|`-type d`                     |Search for directories.|


## Changing ownership of folder
```bash
sudo chown new-owner  filename
```
`new-owner`: Specifies the user name or UID of the new owner

```bash
sudo chown -R $(id -u):$(id -g) demo/
```
`$(id -u)`: is a command substitution in Linux that retrieves the user ID (UID) of the current user. It is used to dynamically insert the UID into a command or script.
- `id` is a command-line utility in Linux that displays the real and effective user and group IDs.
- `-u` is an option for the `id` command that specifies to only display the user ID.
- `-u` is an option for the `id` command that specifies to only display the group ID.

## Jounalctl investigate

```
journalctl -xb -p err..alert
```

The `journalctl -xb -p err..alert` command is used to view, filter, and analyze log data recorded by the systemd journal in Linux. Let's break down the command:

- `journalctl`: This is the base command used to interact with the systemd journal. The systemd journal is a system service that collects and manages system logs. The journal stores logs in a binary format, allowing for faster searching and filtering than text-based logs [itslinuxfoss.com](https://itslinuxfoss.com/use-linux-journalctl-command/).

- `-xb`: This option tells `journalctl` to show logs from the current boot (`-b`) and to display extra information (`-x`) about each log entry if available. Using `-b` with no argument refers to the current boot. You can specify a previous boot by using `-b -1`, `-b -2`, etc. [docs.buddy.red](https://docs.buddy.red/docs/linux/linux-command/journalctl/), [wangchujiang.com](https://wangchujiang.com/linux-command/c/journalctl.html).

- `-p err..alert`: This option filters the log output by priority, showing only log messages with a priority level from "error" to "alert". The priority levels in ascending order are: debug, info, notice, warning, err, crit, alert, and emerg. Therefore, this command will show only the more severe messages. If a single priority level is specified (e.g., `-p err`), then all messages with that priority and higher (i.e., more severe) are shown
- 
### Filtering by Unit
```
journalctl -u nginx.service -u php-fpm.service --since today
```
# config Terminal css

program config either reside in `/etc/[program_name]` or `~/.config/[program_name]`
`nano ~/.config/gtk-3.0/gtk.css`

```css
 VteTerminal,
 TerminalScreen,
 vte-terminal {
     padding: 10px 10px 10px 10px;
     -VteTerminal-inner-border: 10px 10px 10px 10px;
 }
```
