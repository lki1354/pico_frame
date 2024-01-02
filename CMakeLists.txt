cmake_minimum_required(VERSION 3.12)

# Change your executable name to something creative!
set(NAME pico_frame) # <-- Name your project/executable here!

include(pimoroni_pico_import.cmake)
include(pico_sdk_import.cmake)

# Gooey boilerplate
project(${NAME} C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialize the SDK
pico_sdk_init()

# Add your source files
add_executable(${NAME}
    main.cpp # <-- Add source files here!
    #load_image.cpp
)

# Include required libraries
# This assumes `pimoroni-pico` is stored alongside your project
include(common/pimoroni_i2c)
include(common/pimoroni_bus)
#include(libraries/inky_frame/inky_frame)
include(libraries/inky_frame_7/inky_frame_7)
include(drivers/bme68x/bme68x)


# Don't forget to link the libraries you need!
target_link_libraries(
    ${NAME}
    pico_stdlib
    jpegdec
    inky_frame_7
    fatfs
    hardware_pwm
    hardware_spi
    hardware_i2c
    hardware_rtc
    fatfs
    sdcard
    pico_graphics
    pimoroni_i2c
    bme68x
)


pico_enable_stdio_usb(${NAME} 1)

# create map/bin/hex file etc.
pico_add_extra_outputs(${NAME})

target_compile_definitions(${NAME} PUBLIC INKY_FRAME_7)

# Set up files for the release packages
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/${NAME}.uf2
    ${CMAKE_CURRENT_LIST_DIR}/README.md
    DESTINATION .
)

set(CPACK_INCLUDE_TOPLEVEL_DIRECTORY OFF)
set(CPACK_GENERATOR "ZIP" "TGZ")
include(CPack)
