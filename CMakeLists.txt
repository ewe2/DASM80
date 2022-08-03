project(DASM80)
cmake_minimum_required(VERSION 3.18)

# toggle OS def and force file preprosessor scope
# regardless of build system
If (CMAKE_HOST_WIN32)
    set(WINDOWS 1)
elseif (CMAKE_HOST_UNIX)
    set(LINUX 1)
endif()

if (WINDOWS)
    set(WINDOWS "WINDOWS" ON)
elseif (LINUX)
    set(LINUX "LINUX" ON)
endif()

configure_file(
    "${PROJECT_SOURCE_DIR}/DASM80/DASM80Config.h.in"
    "${PROJECT_SOURCE_DIR}/DASM80/DASM80Cconfig.h"
)

if(WINDOWS)
    link_directories("${PROJECT_SOURCE_DIR}/DASM80") 
elseif(LINUX)
    link_directories("${PROJECT_SOURCE_DIR}/DASM80") 
endif()

include_directories(${CMAKE_CURRENT_BINARY_DIR})

add_executable(dasm80 "${PROJECT_SOURCE_DIR}/DASM80/DASM80.c") 

target_link_libraries (dasm80 disasmZ80 memZ80)



set(CMAKE_INSTALL_PREFIX "${PROJECT_NAME}")

