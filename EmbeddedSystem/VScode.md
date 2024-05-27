how to config  c_cpp_properties.json
how to include path [](https://www.youtube.com/watch?v=jcy5TpbXfAY&t=324s)
```cpp
// Name ${workspaceRoot} {workspaceFolder} is folder path names to folder contain this file
// Set paths to arm-none-eabi libraries as it is installed on your system

{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${workspaceFolder}/stm32-cmake-master/**",
                "${workspaceFolder}/stm32-cmake-master/Core/Inc",
                "${workspaceRoot}/stm32-cmake-master/Drivers/STM32F4xx_HAL_Driver/Inc",
                "${workspaceRoot}/stm32-cmake-master/Drivers/CMSIS/Include",
                "${workspaceRoot}/stm32-cmake-master/Drivers/CMSIS/Device/ST/STM32F4xx/Include",

                "/usr/arm-none-eabi/include",
                "/usr/arm-none-eabi/include/c++/13.2.0",
                "/usr/arm-none-eabi/include/machine",
                "/usr/arm-none-eabi/include/newlib-nano",
                "/usr/arm-none-eabi/include/sys",

                "/lib/gcc/arm-none-eabi/13.2.0/include",
                "/lib/gcc/arm-none-eabi/13.2.0/include-fixed"
            ],
            "defines":
            [
                "USE_HAL_DRIVER",
                "STM32F407xx"
            ],
            "cStandard": "c17",
            "cppStandard": "c++20",
            "intelliSenseMode": "linux-gcc-arm",
            "compilerPath": "/usr/bin/arm-none-eabi-gcc",
            "browse":
            {
                "path":
                [
                    "${workspaceRoot}",
                    "${workspaceFolder}/stm32-cmake-master/**",
                    "${workspaceRoot}/stm32-cmake-master/Drivers/STM32F4xx_HAL_Driver/Inc",
                    "${workspaceRoot}/stm32-cmake-master/Drivers/CMSIS/Include",
                    "${workspaceRoot}/stm32-cmake-master/Drivers/CMSIS/Device/ST/STM32F4xx/Include",

                    "/usr/arm-none-eabi/include",
                    "/usr/arm-none-eabi/include/c++/13.2.0",
                    
                    "/lib/gcc/arm-none-eabi/13.2.0/include",
                    "/lib/gcc/arm-none-eabi/13.2.0/include-fixed"
                ],
                "limitSymbolsToIncludedHeaders": true,
                "databaseFilename": "${workspaceRoot}/.vscode/browse.vc.db"
            }
        }
    ],
    "version": 4
}
```