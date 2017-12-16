cmake_minimum_required(VERSION 3.6)
project(litecoin)

set(CMAKE_CXX_STANDARD 11)

SET(CMAKE_CXX_FLAGS_DEBUG "-O0")
SET(CMAKE_C_FLAGS_DEBUG "-O0")

if(CMAKE_BUILD_TYPE MATCHES "Debug")

    set(CMAKE_RULE_MESSAGES off)
    set(CMAKE_COLOR_MAKEFILE on)
    set(CMAKE_VERBOSE_MAKEFILE on)

endif()

add_subdirectory(src)

#include(src/CLionSources.txt)
