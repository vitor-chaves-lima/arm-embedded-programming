cmake_minimum_required(VERSION 3.29)
project(arm_embedded_programming)

enable_language(C ASM)
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

set(STARTUP_FILE ${CMAKE_SOURCE_DIR}/utils/startup_stm32f103xb.s)
set(LINKER_SCRIPT ${CMAKE_SOURCE_DIR}/utils/STM32F103XB_FLASH.ld)

set(HAL_INC_DIR "$ENV{CubeF1}/Drivers/STM32F1xx_HAL_Driver/Inc")
set(HAL_SRC_DIR "$ENV{CubeF1}/Drivers/STM32F1xx_HAL_Driver/Src")
set(CMSIS_INC_DIR "$ENV{CubeF1}/Drivers/CMSIS/Include")
set(CMSIS_DEVICE_INC_DIR "$ENV{CubeF1}/Drivers/CMSIS/Device/ST/STM32F1xx/Include")

add_subdirectory(blink)
add_subdirectory(binary_counter)
add_subdirectory(display7_seg)
add_subdirectory(keyboard_counter)
add_subdirectory(push_button)