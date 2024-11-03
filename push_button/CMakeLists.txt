set(HAL_SOURCES
		${HAL_SRC_DIR}/stm32f1xx_hal.c
		${HAL_SRC_DIR}/stm32f1xx_hal_rtc_ex.c
		${HAL_SRC_DIR}/stm32f1xx_hal_uart.c
		${HAL_SRC_DIR}/stm32f1xx_hal_rcc.c
		${HAL_SRC_DIR}/stm32f1xx_hal_cortex.c
		${HAL_SRC_DIR}/stm32f1xx_hal_pwr.c
		${HAL_SRC_DIR}/stm32f1xx_hal_tim_ex.c
		${HAL_SRC_DIR}/stm32f1xx_hal_flash.c
		${HAL_SRC_DIR}/stm32f1xx_hal_rcc_ex.c
		${HAL_SRC_DIR}/stm32f1xx_hal_rtc.c
		${HAL_SRC_DIR}/stm32f1xx_hal_gpio_ex.c
		${HAL_SRC_DIR}/stm32f1xx_hal_gpio.c
		${HAL_SRC_DIR}/stm32f1xx_hal_dma.c
		${HAL_SRC_DIR}/stm32f1xx_hal_flash_ex.c
		${HAL_SRC_DIR}/stm32f1xx_hal_tim.c
)

set(SOURCES
		${HAL_SOURCES}
		${CMAKE_CURRENT_SOURCE_DIR}/src/stm32f1xx_hal_msp.c
		${CMAKE_CURRENT_SOURCE_DIR}/src/system_stm32f1xx.c
		${CMAKE_CURRENT_SOURCE_DIR}/src/stm32f1xx_it.c
		${CMAKE_CURRENT_SOURCE_DIR}/src/main.c
)

set(EXECUTABLE push_button.elf)

add_executable(${EXECUTABLE} ${SOURCES} ${STARTUP_FILE})

target_compile_definitions(${EXECUTABLE} PRIVATE
		-DUSE_HAL_DRIVER
		-DSTM32F103xB
		-DHSE_VALUE=8000000
)

target_include_directories(${EXECUTABLE} PRIVATE
		./inc
		${HAL_INC_DIR}
		${CMSIS_INC_DIR}
		${CMSIS_DEVICE_INC_DIR}
)

target_compile_options(${EXECUTABLE} PRIVATE
		-mcpu=cortex-m3
		-mthumb
		-mlittle-endian
		-mthumb-interwork
		-std=gnu99
		-O2
		-Wall
		-Wextra
		-Wunused-parameter
		-Wno-sequence-point
		-Wl,-T${LINKER_SCRIPT}
		$<$<CONFIG:Debug>:-Og>
)

target_link_options(${EXECUTABLE} PRIVATE
		-T${LINKER_SCRIPT}
		-mcpu=cortex-m3
		-mthumb
		-mlittle-endian
		-mthumb-interwork
		-lm -lc -lnosys
)

add_custom_command(TARGET ${EXECUTABLE}
		POST_BUILD
		COMMAND arm-none-eabi-size ${EXECUTABLE})

add_custom_command(TARGET ${EXECUTABLE}
		POST_BUILD
		COMMAND arm-none-eabi-objcopy -O ihex ${EXECUTABLE} blink.hex
		COMMAND arm-none-eabi-objcopy -O binary ${EXECUTABLE} blink.bin)
