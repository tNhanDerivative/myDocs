
```bash
(comm --nocheck-order -23 <(history | cut -c  8-) ~/.bash_history)> cau01.sh

```

`comm` to compare the output of the `history` command with the contents of the `.bash_history` file. The `cut -c 8-` part removes the line numbers from the `history` output to match the format of the `.bash_history` file. The `-23` option tells `comm` to suppress lines unique to the second file and lines identical in both files, effectively showing only lines unique to the first file, which would be the commands executed in the current session but not saved in the `.bash_history` file yet

[lệnh comm](https://www.geeksforgeeks.org/comm-command-in-linux-with-examples/)