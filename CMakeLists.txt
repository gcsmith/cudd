################################################################################
# cudd
################################################################################

cmake_minimum_required(VERSION 3.11 FATAL_ERROR)
cmake_policy(SET CMP0048 NEW)
project(cudd VERSION 3.0.0)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING
        "Build type: None Debug Release RelWithDebInfo MinSizeRel" FORCE)
endif()

option(ENABLE_SHARED "Build shared library" ON)
option(ENABLE_DDDMP "Build shared library" ON)
option(ENABLE_OBJ "Build shared library" ON)
option(ENABLE_NANOTRAV "Build nanotrav executable" ON)

if(ENABLE_SHARED)
    set(CMAKE_POSITION_INDEPENDENT_CODE ON)
endif()

include_directories(. cudd dddmp epd st mtr util)

################################################################################
# Build subdirectories
################################################################################

add_subdirectory(cudd)
add_subdirectory(epd)
add_subdirectory(mtr)
add_subdirectory(st)
add_subdirectory(util)

set(cudd_libs
    $<TARGET_OBJECTS:cudd_core>
    $<TARGET_OBJECTS:cudd_epd>
    $<TARGET_OBJECTS:cudd_mtr>
    $<TARGET_OBJECTS:cudd_st>
    $<TARGET_OBJECTS:cudd_util>
)

if(ENABLE_DDDMP)
    add_subdirectory(dddmp)
    list(APPEND cudd_libs $<TARGET_OBJECTS:cudd_dddmp>)
endif()
if(ENABLE_OBJ)
    add_subdirectory(cplusplus)
    list(APPEND cudd_libs $<TARGET_OBJECTS:cudd_obj>)
endif()
if(ENABLE_NANOTRAV)
    add_subdirectory(nanotrav)
endif()

################################################################################
# Link libcudd
################################################################################

if(ENABLE_SHARED)
    add_library(cudd SHARED ${cudd_libs})
else()
    add_library(cudd STATIC ${cudd_libs})
endif()

