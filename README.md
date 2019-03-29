# About

This project is used to develop applications for the STM32 - ST's ARM Cortex-Mx MCUs. It uses cmake and GCC, along with ChibiOS 18.2.1.

## Requirements

* cmake >= 3.0
* GCC toolchain with newlib (optional).

## Project contains

* CMake common toolchain file, that configures cmake to use the arm toolchain.
* CMake toolchain file that can generate a tunable linker script
* CMake STM32 family-specific toolchain file, that configures family-specific parameters.
* CMake modules to find and configure ChibiOS components.
* CMake project template.

## Examples
* `stm32-chibios-rt-f0` - blink led using ChibiOS/RT from STM32F0 Discovery. 

# Usage

First of all you need to configure toolchain and libraries, you can do this by editing `gcc_stm32.cmake` or, preferably, by passing it through the command line.

## Configuration

* `TOOLCHAIN_PREFIX` - where toolchain is located, **default**: `/usr`
* `TARGET_TRIPLET` - toolchain target triplet, **default**: `arm-none-eabi`
* `STM32_CHIP` - STM32 device code, e.g. `STM32F407VG` or `STM32F103VG`
* `STM32_FAMILY` - STM32 family (F0, F1, F4, etc.) currently, F0, F1, F2, F4, F7, L0, L1 and L4 families are supported. **Note:** If `STM32_CHIP` variable is set, `STM32_FAMILY` is optional.

To use the toolchain, you'll need to copy contents of the `cmake` folder into cmake's modules path, or use the `CMAKE_MODULE_PATH` variable.

## Common usage

    cmake -DSTM32_CHIP=<chip> -DCMAKE_TOOLCHAIN_FILE=<path_to_gcc_stm32.cmake> -DCMAKE_BUILD_TYPE=Debug <path_to_source_dir>

Where `<chip>` is the STM32 chip name (e.g. `STM32F100C8`, `STM32F407IG`).

This command will generate Makefile for project. For a `Release` build, change `CMAKE_BUILD_TYPE`.

The script will try to detect chip parameters automatically from the chip name (type, flash/ram size), or, you can set these directly with these variables:

* `STM32_CHIP_TYPE` - family-dependent chip type. Global variable `STM32_CHIP_TYPES` contains list of valid types for current family (e.g `207xG`)
* `STM32_FLASH_SIZE` - chip flash size (e.g. 64K)
* `STM32_RAM_SIZE` - chip RAM size (e.g. 4K)

## Building

* To build elf file: `make`
* To build .hex: `make <project name>.hex`
* To build .bin: `make <project name>.bin`

## Linker script & variables

You can use cmake variables below to tune the generated linker. To specify a custom linker script, set `STM32_LINKER_SCRIPT` (you can still use these variables in your custom script).

* `STM32_FLASH_ORIGIN` - Start address of flash (**default**: 0x08000000)
* `STM32_RAM_ORIGIN` - Start address of RAM (**default**: 0x20000000)
* `STM32_FLASH_SIZE` - Flash size (**default**: from chip name)
* `STM32_RAM_SIZE` - RAM size (**default**: from chip name)
* `STM32_MIN_STACK_SIZE` - Minimum stack size for error detection at link-time (**default**: 512 bytes)
* `STM32_MIN_HEAP_SIZE` - Minimum heap size for error detection at link-time (**default**: 0 bytes)
* `STM32_CCRAM_ORIGIN` - Start address of Core-Coupled RAM (**default**: 0x10000000)
* `STM32_CCRAM_SIZE` - Core-Coupled RAM size (**default**: 64 KiB)

## Useful cmake macros

* `STM32_GET_CHIP_TYPE(CHIP CHIP_TYPE)` - gets chip type from chip name.
* `STM32_GET_CHIP_PARAMETERS(CHIP FLASH_SIZE RAM_SIZE CCRAM_SIZE)` - gets chip ram/flash size from chip name.
* `STM32_SET_FLASH_PARAMS(TARGET ...)` - sets chip flash/ram parameters for target.
* `STM32_SET_CHIP_DEFINITIONS(TARGET CHIP_TYPE)` - sets chip family and type-specific compiler flags for target.
* `STM32_SET_TARGET_PROPERTIES(TARGET)` - sets all needed parameters and compiler flags for target.
* `STM32_GENERATE_LIBRARIES(NAME SOURCES LIBRARIES)` - generates libraries for all chip types in family. Resulting libraries stored in LIBRARIES and have names in ${NAME}_${FAMILY}_${CHIP_TYPE} format.

# ChibiOS Support

This project also supports ChibiOS v18.2.1.

CMake modules for ChibiOS can find specified ChibiOS components parse halconf.h file.

See project `stm32-chibios-rt-f0` for example usage.


