
# Overview

Check out this youtube tutorial [link here](https://www.youtube.com/watch?v=lZp9L7Ij4Yo&t=421s) with this markdown tutorial: [link here](https://github.com/espressif/esp-toolchain-docs/blob/main/qemu/esp32/README.md) 
Dùng video youtube để rút ngắn lại tutorial

# Install Qemu for Esp-idf
[Espressif Document page](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/tools/qemu.html#prerequisites "Permalink to this headline")

To use QEMU with `idf.py` you first need to install the above-mentioned fork of QEMU. ESP-IDF provides pre-built binaries for x86_64 and arm64 Linux and macOS, as well as x86_64 Windows. If you are using this platform, you can install the pre-built binaries with the following command:
```python
python $IDF_PATH/tools/idf_tools.py install qemu-xtensa qemu-riscv32
```

After installing QEMU, make sure it is added to PATH by running `. ./export.sh` in the IDF directory.

If you are using a different platform, you need to build QEMU from source. Refer to official QEMU documentation for instructions.

# Compiling the ESP-IDF program to emulate

QEMU for ESP32 target is ready, it already includes the first stage bootloader, located on the ROM on the real chip, which is mainly responsible for initializing the peripherals, like the UART and, more importantly, the SPI Flash. The latter **must** contain the second stage bootloader and the program to run.

Thus, in this section, we are going to create a flash image that combines the (second stage) bootloader, the partition table and the application to run. This can be done using `esptool.py merge_bin` command, supported in `esptool.py` 3.1 or later. 

- Let's suppose that the ESP-IDf project has just been compiled successfully, the following commands will create that flash image:

```bash
cd build
esptool.py --chip esp32 merge_bin --fill-flash-size 4MB -o flash_image.bin @flash_args
```

Here, `flash_args` is a file generated by ESP-IDF build system in the build directory, it contains the list of names of binary files and corresponding flash addresses. `merge_bin` command takes this list and creates the binary image of the whole flash. `--fill-flash-size 4MB` argument specifies the total flash size.

ESP32 target in QEMU supports flash of size 2, 4, 8 and 16MB, creating an image with any other size will result in an error.

**Notes**

* For "Secure Boot" feature in ESP-IDF, we recommend separate command to flash bootloader and hence `flash_args` file do not have corresponding entry. However, you may modify `flash_args` file to add entry for `bootloader.bin` as per below:

    ```
    0x1000 bootloader/bootloader.bin
    ```

* It is also possible to use `esptool.py` to "flash" the application into QEMU, but QEMU needs to be started with the right strapping mode. [See Bootstrapping Mode section below for more info](#specifying-bootstrapping-mode).

# Run QEMU on vscode

## install extension
Install NATIVE DEBUG extension for vscode

## Config for NATIVE DEBUG extension
open launch.json. Because we installed the extension now there's a button **"Add Configuration"** will appear 
and we will choose **ESP-IDF: Debug ..."** option

```json
```


