set(CMAKE_TOOLCHAIN_FILE /path to/stm32-cmake/cmake/gcc_stm32.cmake)
set(TOOLCHAIN_PREFIX /usr)
set(CMAKE_MODULE_PATH /path to/stm32-cmake/cmake/)
set(TARGET_TRIPLET "arm-none-eabi")

set(CHIBIOS_ROOT /path to/chibios1821)

set(STM32_CHIP STM32L432KC)
set(STM32_FAMILY L4)


set(STM32_FLASH_ORIGIN "0x08000000")
set(STM32_RAM_ORIGIN   "0x20000000")
set(STM32_MIN_STACK_SIZE "0x400")
set(STM32_MIN_HEAP_SIZE "0x200")

set(USE_FPU "soft")

FIND_PACKAGE(ChibiOS 18 COMPONENTS rt hal REQUIRED)

SET(STM32_LINKER_SCRIPT ${ChibiOS_LINKER_SCRIPT})

PROJECT(stm32_l432)

CMAKE_MINIMUM_REQUIRED(VERSION 3.6)
ENABLE_LANGUAGE(ASM)


include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${ChibiOS_INCLUDE_DIRS}
    board

)

set(PROJECT_SOURCES
    main.c
    board/board.c
    board/board.h
    halconf.h
    mcuconf.h
    chconf.h
)



ADD_EXECUTABLE(${CMAKE_PROJECT_NAME} ${PROJECT_SOURCES} ${ChibiOS_SOURCES})
TARGET_LINK_LIBRARIES(${CMAKE_PROJECT_NAME})

STM32_SET_TARGET_PROPERTIES(${CMAKE_PROJECT_NAME})
STM32_ADD_HEX_BIN_TARGETS(${CMAKE_PROJECT_NAME})
