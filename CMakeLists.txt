cmake_minimum_required(VERSION 3.10)

project(modbus LANGUAGES C VERSION 3.1.6)

file(GLOB HEADERS "src/*.h")
file(GLOB SOURCES "src/*.c")

# A temporary hack to get the versions configured correctly.

set(LIBMODBUS_VERSION_MAJOR 3)
set(LIBMODBUS_VERSION_MINOR 1)
set(LIBMODBUS_VERSION_MICRO 6)

configure_file("src/win32/config.h.win32" "src/config.h" COPYONLY)
configure_file("src/modbus-version.h.in" "src/modbus-version.h" @ONLY)

add_library(libmodbus
    SHARED
        ${SOURCES}
        ${HEADERS}
        "src/config.h")

target_include_directories(libmodbus
    PUBLIC
        "${CMAKE_CURRENT_BINARY_DIR}/src")

target_compile_definitions(libmodbus
    PRIVATE
        "HAVE_CONFIG_H"
        "DLLBUILD"
        "_CRT_SECURE_NO_DEPRECATE=1"
        "_CRT_NONSTDC_NO_DEPRECATE=1")

if(WIN32)
    target_link_libraries(libmodbus
        PRIVATE
            wsock32
            ws2_32)
endif()
