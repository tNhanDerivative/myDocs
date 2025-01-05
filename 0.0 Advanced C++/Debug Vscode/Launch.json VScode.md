

# Source
[Youtube Debugging C++ & CMake in VSCode](https://youtu.be/Qng2RW_bjS8)

[file `launch.json` mẫu](https://vector-of-bool.github.io/docs/vscode-cmake-tools/debugging.html#debugging-with-cmake-tools-and-launch-json)

## Vscode variables:
-`${workspaceFolder}`: the path of the folder opened in VS Code
- `${file}`: Full path of current file
- `${fileDirname}`: Directory containing current file
- `${fileBasenameNoExtension}`: Current filename without extension

## `Launch.json` setting cần chú ý
The path of the executable you want to debug example `${workspaceFolder}/build/ClientApp/ClientApp`
`${command:cmake.launchTargetPath}` use your selection by `CMakeTools`
```json
"program": "${command:cmake.launchTargetPath}",
```

Command line argument to add to the program: 
```json
"args": "--hello"
```
working directory of target
```json
"cwd": "${workspaceFolder}/build",
```


```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            // Resolved by CMake Tools:
            "program": "${command:cmake.launchTargetPath}",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [
                {
                    // add the directory where our target was built to the PATHs
                    // it gets resolved by CMake Tools:
                    "name": "PATH",
                    "value": "$PATH:${command:cmake.launchTargetDirectory}"
                },
                {
                    "name": "OTHER_VALUE",
                    "value": "Something something"
                }
            ],
            "externalConsole": true,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}
```