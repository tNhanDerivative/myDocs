# Source
[github tutorial](https://github.com/kirthandev/MIUI-Debloater-official)
# Install Java SE development kit

# Install ADB
**Ubuntu/Debian**
```sh
sudo apt install adb fastboot
```

**Fedora**
```sh
sudo dnf install android-tools
```

**For Arch Linux:**

```
sudo pacman -S android-tools
```

# Enable Developer Option on phone


# Turn on USB Debugging
To turn USB Debugging on, go to your device **Settings** → **Additional Settings** → **Developer Option**. Locate **USB Debugging** and toggle it on. 

# Use ADB
Now, connect your device to your computer and start the adb server
```sh
sudo adb start-server
```

allow USB debugging on Phone => check if the device is connected or not using the following:

```
adb devices
```

