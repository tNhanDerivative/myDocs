
# Source

[Youtube Attach debugger to a running Node process](https://www.youtube.com/watch?v=WTAKR1du03I)

Here are the key points about attaching a debugger to VSCode:

## Configuring C++ `GDB Attach`

1. Create a launch configuration in `launch.json` for attach debugging [1]:

```json
{
  "name": "Attach to Process",
  "type": "cppdbg",
  "request": "attach",
  "program": "${workspaceFolder}/path/to/executable",
  "processId": "${command:pickProcess}",
  "MIMode": "gdb",
  "setupCommands": [
    {
      "description": "Enable pretty-printing for gdb",
      "text": "-enable-pretty-printing",
      "ignoreFailures": true
    }
  ]
}
```

2. The `program` field specifies the path to the executable you want to attach to [1].

3. Use `${command:pickProcess}` for the `processId` to select a running process [1].

## Attaching the Debugger

1. Start debugging by selecting the "Attach to Process" configuration from the debug dropdown [3].

2. VSCode will show a list of available processes to attach to [3].

3. Select the process you want to debug from the list [3].

4. The debugger will now be attached to the selected process [3].

## Key Considerations

- Make sure your executable has been built with debugging symbols enabled [3].

- You may need to set breakpoints in your code before attaching the debugger [3].

- The `setupCommands` array allows you to configure GDB before attaching [1].

- You can customize additional options like external console, symbol paths, etc. in the launch configuration [1].

By following these steps, you should be able to successfully attach a debugger to a running process using VSCode. Let me know if you need any clarification or have additional questions!

Citations:
[1] https://code.visualstudio.com/docs/cpp/launch-json-reference
[2] https://stackoverflow.com/questions/73148147/why-vscode-debugger-require-program-property-when-i-attach-gdb-to-a-running-pr
[3] https://ardupilot.org/dev/docs/debugging-with-gdb-using-vscode.html
[4] https://www.reddit.com/r/vscode/comments/1c7kne8/configuring_tasksjson_and_launchjson_for/
[5] https://community.gamedev.tv/t/how-to-attach-debugger-to-vscode/182607
[6] https://github.com/microsoft/vscode-cpptools/issues/6134
[7] https://www.bacpeters.com/2023/02/12/using-gdb-with-gdb-server-within-visual-studio-code/
[8] https://learn.microsoft.com/en-us/answers/questions/1846022/how-to-fix-the-debugger-in-visual-studio-code
[9] https://code.visualstudio.com/docs/cpp/cpp-debug
[10] https://cwiki.apache.org/confluence/display/IMPALA/Debugging+with+GDB+in+VSCode