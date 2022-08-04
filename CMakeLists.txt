project(dasm80)
cmake_minimum_required(VERSION 3.18)

# needed to get cmake to use C++ and catch _s versions of string.h
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

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
    DASM80/DASM80Config.h.in
    DASM80/DASM80Cconfig.h
)

if(WINDOWS)
    link_directories(DASM80) 
elseif(LINUX)
    link_directories(DASM80) 
endif()

include_directories(dasm80 DASM80)

add_executable(dasm80
    DASM80/DASM80.c
    DASM80/disasZ80.c
    DASM80/memZ80.c
)

set(CMAKE_INSTALL_PREFIX "${PROJECT_NAME}")

