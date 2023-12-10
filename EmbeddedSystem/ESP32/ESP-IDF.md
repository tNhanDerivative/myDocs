# Binary Toolchain
https://gitdemo.readthedocs.io/en/latest/linux-setup.html#step-0-prerequisites

[ Hướng dẫn trên espressif](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/linux-macos-setup.html#get-started-set-up-env)


# ESP-IDF
## Step 1. Install Prerequisites

FEDORA
`sudo dnf install git wget flex bison gperf python3 python3-pip python3-venv cmake ninja-build ccache libffi openssl dfu-util libusb`
DEBIAN
`sudo apt-get install git wget flex bison gperf python3 python3-pip python3-venv cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0`

## Step 2. Get ESP-IDF

```bash
mkdir -p ~/code/tools/esp
cd ~/code/tools/esp
git clone --recursive https://github.com/espressif/esp-idf.git
```
`--recursive` option is to install all the sub repositories
Note the --recursive option! If you have already cloned ESP-IDF without this option, run another command to get all the submodules:
```bash
cd ~/code/tools/esp/esp-idf
git submodule update --init
```

## Step 3. Set up the tools

Aside from the ESP-IDF, you also need to install the tools used by ESP-IDF, such as the compiler, debugger, Python packages, etc, for projects supporting ESP32.
```bash
cd ~/code/tools/esp/esp-idf
./install.sh esp32
```
In order to install tools for all supported targets
```bash
cd ~/code/tools/esp/esp-idf
./install.sh all
```

## Step 4. Set up the environment variables

The installed tools are not yet added to the PATH environment variable. To make the tools usable from the command line, some environment variables must be set. ESP-IDF provides another script which does that.
`. $HOME/code/tools/esp/esp-idf/export.sh`

Copy and paste the following command to your shell’s profile (`.profile`, `.bashrc`, `.zprofile`, etc.)
`alias get_idf='. $HOME/esp/esp-idf/export.sh'`

## Start a project
First to export  environment variables for ESP-IDF use this command we already alias in `.bashrc`
`get_idf`

# Attention
Only after you set-target you can include thing in vscode

`idf.py set-target esp32`
now configure your project
`idf.py menuconfig`

## Build the Project
Build the project by running:

`idf.py build`

