

# Vscode create project
Open Command Palette by pressing   `F1`
Click on `ESP-IDF: new project`



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