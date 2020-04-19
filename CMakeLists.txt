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

################################################################################
# Generate config.h
################################################################################

include(CheckFunctionExists)
check_function_exists(getrlimit HAVE_GETRLIMIT)
check_function_exists(getrusage HAVE_GETRUSAGE)
check_function_exists(powl HAVE_POWL)
check_function_exists(sysconf HAVE_SYSCONF)

include(CheckIncludeFiles)
check_include_files(assert.h HAVE_ASSERT_H)
check_include_files(inttypes.h HAVE_INTTYPES_H)
check_include_files(stddef.h HAVE_STDDEF_H)
check_include_files(stdint.h HAVE_STDINT_H)
check_include_files(stdlib.h HAVE_STDLIB_H)
check_include_files(string.h HAVE_STRING_H)
check_include_files(sys/resource.h HAVE_SYS_RESOURCE_H)
check_include_files(sys/time.h HAVE_SYS_TIME_H)
check_include_files(sys/times.h HAVE_SYS_TIMES_H)
check_include_files(sys/wait.h HAVE_SYS_WAIT_H)
check_include_files(unistd.h HAVE_UNISTD_H)
check_include_files("stdlib.h;stdarg.h;string.h;float.h" STDC_HEADERS)

include(CheckSymbolExists)
check_symbol_exists(INFINITY math.h HAVE_IEEE_754)

include(CheckTypeSize)
check_type_size(int SIZEOF_INT)
check_type_size(long SIZEOF_LONG)
check_type_size("long double" SIZEOF_LONG_DOUBLE)
check_type_size("void*" SIZEOF_VOID_P)

include(TestBigEndian)
test_big_endian(WORDS_BIGENDIAN)

configure_file(config.h.cmake config.h)

################################################################################
# Build subdirectories
################################################################################

include_directories(. cudd dddmp epd st mtr util ${PROJECT_BINARY_DIR})

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

