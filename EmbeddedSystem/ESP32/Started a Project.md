

# Setup vscode

Open Command Palette by pressing   `F1`
Click on `ESP-IDF: new project`
to jump into code and disable vscode include error. Add this file into `.vscode` folder
c_cpp_properties.json
```cpp
{
    "configurations": [
        {
            "name": "ESP-IDF",
            "compilerPath": "",
            "includePath": [
                "${config:idf.espIdfPath}/components/**",
                "${config:idf.espIdfPathWin}/components/**",
                "${config:idf.espAdfPath}/components/**",
                "${config:idf.espAdfPathWin}/components/**",
                "${workspaceFolder}/**"
            ],
            "browse": {
                "path": [
                    "${config:idf.espIdfPath}/components",
                    "${config:idf.espIdfPathWin}/components",
                    "${config:idf.espAdfPath}/components/**",
                    "${config:idf.espAdfPathWin}/components/**",
                    "${workspaceFolder}/**",
                    "${workspaceFolder}"
                ],
                "limitSymbolsToIncludedHeaders": false
            }
        }
    ],
    "version": 4
}
```



# Terminal Create project

```bash
idf.py create-project <project_name>
idf.py menuconfig
```
config the project then build
```bash
idf.py build
```
flash on esp32
```bash
idf.py build flash monitor
```

