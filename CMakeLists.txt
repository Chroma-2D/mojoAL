cmake_minimum_required(VERSION 3.0)

project(mojoAL)

set(DEFAULT_BUILD_SHARED ON)
set(DEFAULT_BUILD_STATIC ON)

# FIXME: when i care about tests write cmakelists there
# set(DEFAULT_BUILD_TESTS OFF)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/scripts)

find_package(SDL2 REQUIRED)

if(NOT SDL2_FOUND)
    message(FATAL_ERROR "SDL2 could not be found.")
endif(NOT SDL2_FOUND)

if(NOT SDL2MAIN_LIBRARY)
    message(WARNING "SDL2main library could not be found.")
    SET(SDL2MAIN_LIBRARY "")
endif(NOT SDL2MAIN_LIBRARY)

include_directories(${SDL2_INCLUDE_DIR})
link_libraries(${SDL2MAIN_LIBRARY} ${SDL2_LIBRARY})

option(BUILD_SHARED "Build mojoAL as a shared library" ${DEFAULT_BUILD_SHARED})
option(BUILD_STATIC "Build mojoAL as a static library" ${DEFAULT_BUILD_STATIC})

# FIXME: when i care about tests write cmakelists there
# option(BUILD_TESTS "Build mojoAL tests" ${DEFAULT_BUILD_TESTS})

include_directories(include)
add_definitions("-Wall -pedantic")
add_subdirectory(src)

# FIXME: when i care about tests write cmakelists there
# if(BUILD_TESTS)
#     add_subdirectory(tests)
# endif(BUILD_TESTS)