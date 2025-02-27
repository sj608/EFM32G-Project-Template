# EFM32G Project Template

This repository is my personal project template to get the development of EFM32G family development up and running. Few tweaks need to be made for the repo to work.

## Build tools
Install arm-none-eabi tool chain from ARM website. After the installation, move the bin files with prefix `arm-none-eabi-` to system bin directory or any directory. Then add the path to the bin files to `$PATH` variable. Make sure to logout and login.

Edit Bin util and arm toolchain directory path for the host platform inside cmake/arm-none-eabi.cmake
```
set(ARM_TOOLCHAIN_DIR "path/to/binutil/") # set arm toolchain directory path
set(BINUTILS_PATH ${ARM_TOOLCHAIN_DIR}) # set binary utility directory path
```

Cross-compiling setting is configured by invoking the following cmake command
```
cmake -B build -D CMAKE_TOOLCHAIN_FILE=cmake/arm-none-eabi.cmake
```
Included following lines to suppress error regarding compiler configs
```
SET (CMAKE_C_COMPILER_WORKS 1)
SET (CMAKE_CXX_COMPILER_WORKS 1)
```

To build
```
cd build
cmake --build .
```

## Linker Options
`-specs` options uses spec file specified by the string the spec files are inside the ARM lib folder `/usr/local/gcc_arm/ARM/arm-none-eabi/lib`

`-Map=${PROJECT_NAME}.map` this option requires `-Wl,` in front of it.

The issue regarding re-declaration of type is resolved by downgrading the CMSIS pack from 5-5.9.0 to 4.5.0

## JLinkEXE
Programming flow command steps
```
connect
erase
loadbin /directory/to/binfile ...... /Users/sj/Documents/projects/efm-blink/build/EFM-Blink.bin 0
r  - means reset
g  - means go
```

## GDB debugger
Steps to running debugging environment
1. open two terminal
2. Run JLinkGDBServerCL -device EFM32G890F128
3. On another terminal arm-none-eabi-gdb
   - target remote localhost:portnumber/shown/on/JLinkGDBServerCL
   - load, this programs the target and starts from the reset_handler

## References
Example github repo 1 is [here](https://github.com/cortexm/baremetal)

Example github repo 2 is [here](https://mcuoneclipse.com/2021/05/04/visual-studio-code-for-c-c-with-arm-cortex-m-part-2/)

Good reference is [here](https://dev.to/younup/cmake-on-stm32-the-beginning-3766)

CMake Referecnce is [here](https://cmake.org/cmake/help/latest/manual/cmake.1.html#introduction-to-cmake-buildsystems)

Adding arm-none-eabi-gcc package to path [here](https://gist.github.com/joegoggins/7763637)

CMake Project Structure is [here](https://github.com/embeddedartistry/cmake-project-skeleton)

GCC-GNU [compiler](https://gcc.gnu.org/onlinedocs/gcc/ARM-Options.html) and [linker](https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html) options

Uploading Through J-Link steps [here](https://community.silabs.com/s/article/using-jlink-commander-to-program-flash?language=en_US)

GDB setup [interrupt-blog](https://interrupt.memfault.com/blog/gdb-for-firmware-1)

GDBGUI browser frontend [here](https://www.gdbgui.com/guides/)