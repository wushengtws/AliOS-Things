# Top-level cmake file for building MicroPython on ESP32.

cmake_minimum_required(VERSION 3.12)

# Set the location of this port's directory.
set(MICROPY_PORT_DIR ${CMAKE_SOURCE_DIR})

# Set the location of AOS components directory.
if(NOT AOS_COMPONENTS_DIR)
    get_filename_component(AOS_COMPONENTS_DIR ${MICROPY_PORT_DIR}/../../../ ABSOLUTE)
endif()

# Set the location of AOS components directory.
if(NOT HAAS_ENGINE_DIR)
    get_filename_component(HAAS_ENGINE_DIR ${MICROPY_PORT_DIR}/../../ ABSOLUTE)
endif()

# Set the board if it's not already set.
if(NOT MICROPY_BOARD)
    set(MICROPY_BOARD GENERIC)
endif()

# Set the board directory and check that it exists.
if(NOT MICROPY_BOARD_DIR)
    set(MICROPY_BOARD_DIR ${MICROPY_PORT_DIR}/boards/${MICROPY_BOARD})
endif()
if(NOT EXISTS ${MICROPY_BOARD_DIR}/mpconfigboard.cmake)
    message(FATAL_ERROR "Invalid MICROPY_BOARD specified: ${MICROPY_BOARD}")
endif()

# Define the output sdkconfig so it goes in the build directory.
set(SDKCONFIG ${CMAKE_BINARY_DIR}/sdkconfig)

# Include board config; this is expected to set SDKCONFIG_DEFAULTS (among other options).
include(${MICROPY_BOARD_DIR}/mpconfigboard.cmake)

# Include build options for port extend modules.
include(${HAAS_ENGINE_DIR}/modules/config.cmake)

# Concatenate all sdkconfig files into a combined one for the IDF to use.
file(WRITE ${CMAKE_BINARY_DIR}/sdkconfig.combined.in "")
foreach(SDKCONFIG_DEFAULT ${SDKCONFIG_DEFAULTS})
    file(READ ${SDKCONFIG_DEFAULT} CONTENTS)
    file(APPEND ${CMAKE_BINARY_DIR}/sdkconfig.combined.in "${CONTENTS}")
endforeach()
configure_file(${CMAKE_BINARY_DIR}/sdkconfig.combined.in ${CMAKE_BINARY_DIR}/sdkconfig.combined COPYONLY)
set(SDKCONFIG_DEFAULTS ${CMAKE_BINARY_DIR}/sdkconfig.combined)

# Include main IDF cmake file and define the project.
include($ENV{IDF_PATH}/tools/cmake/project.cmake)

if(MICROPY_PY_LVGL)
    # Include LVGL component, ignore KCONFIG
    idf_build_set_property(LV_MICROPYTHON 1)
    idf_build_set_property(LV_ESPIDF_ENABLE 0)
    set(LVGL_DIR ${AOS_COMPONENTS_DIR}/little_ui/3rdparty/lvgl)
    set(LV_CONFIG_DIR ${AOS_COMPONENTS_DIR}/little_ui)
    idf_build_component(${AOS_COMPONENTS_DIR}/little_ui/3rdparty/lvgl)
    idf_build_component(${AOS_COMPONENTS_DIR}/little_ui/driver/axp192)
    idf_build_component(${AOS_COMPONENTS_DIR}/little_ui/driver/i2c_manager)
    idf_build_component(${AOS_COMPONENTS_DIR}/little_ui/driver/lvgl_esp32_drivers)
    idf_build_set_property(COMPILE_DEFINITIONS "-DLV_KCONFIG_IGNORE" APPEND)
    separate_arguments(LV_CFLAGS_ENV UNIX_COMMAND $ENV{LV_CFLAGS})
    list(APPEND LV_CFLAGS ${LV_CFLAGS_ENV})
    idf_build_set_property(COMPILE_DEFINITIONS "${LV_CFLAGS}" APPEND)
endif()

project(micropython)
