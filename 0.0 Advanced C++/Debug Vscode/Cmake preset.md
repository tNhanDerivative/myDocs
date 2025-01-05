

Specify đường dẫn đến binary file 

```json
"binaryDir": "${sourceDir}/out/build/${presetName}",
```



```json
{
  "version": 5,
  "cmakeMinimumRequired": {
    "major": 3,
    "minor": 23,
    "patch": 0
  },
  "configurePresets": [
    {
      "name": "debug-redhat-x86",
      "displayName": "debug-redhat-x86",
      "description": "debug-redhat-x86",
      "generator": "Ninja",
      "binaryDir": "${sourceDir}/build/debug-redhat-x86",
      "cacheVariables": {
        "CMAKE_PREFIX_PATH": "/path/to/your/package"
      },
      // ... other configuration options ...
    }
  ],
  // ... rest of your CMakePresets.json ...
}

```

You can add multiple paths by separating them with colons (:) on Linux and semicolon on Window.