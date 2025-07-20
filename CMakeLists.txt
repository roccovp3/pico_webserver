# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(HOSTNAME rocco3)

# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)

# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.1.1)
set(toolchainVersion RISCV_RPI_2_0_0_5)
set(picotoolVersion 2.1.1)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
set(PICO_BOARD pico2_w CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

project(pico_webserver C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1

add_executable(pico_webserver pico_webserver.c lwipopts.h)

add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/lib/picow-http)

pico_set_program_name(pico_webserver "pico_webserver")
pico_set_program_version(pico_webserver "0.1")

# Modify the below lines to enable/disable output over UART/USB
pico_enable_stdio_uart(pico_webserver 1)
pico_enable_stdio_usb(pico_webserver 0)

# Add the standard library to the build
target_link_libraries(pico_webserver
        pico_stdlib)

# Add the standard include files to the build
target_include_directories(pico_webserver PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}
)

# Add any user requested libraries
target_link_libraries(pico_webserver 
        pico_cyw43_arch_lwip_poll
        picow_http
        )

picow_http_gen_handlers(pico_webserver
	${CMAKE_CURRENT_LIST_DIR}/www.yaml
	${CMAKE_CURRENT_LIST_DIR}/web
	${CMAKE_CURRENT_LIST_DIR}/web/index.html
)


pico_add_extra_outputs(pico_webserver)
pico_enable_stdio_uart(pico_webserver 1)
add_compile_definitions(${NAME} PRIVATE
                        CYW43_HOST_NAME=\"${HOSTNAME}\")

