cmake_minimum_required(VERSION 3.15.0)

# For MSVC RUNTIME LIBRARY, need CMP0091=NEW and cmake 3.15+
cmake_policy(SET CMP0091 NEW)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
# Version info
set(ADEPT_VERSION_MAJOR 4)
set(ADEPT_VERSION_MINOR 1)
set(ADEPT_VERSION_PATCH 1)
set(ADEPT_VERSION ${ADEPT_VERSION_MAJOR}.${ADEPT_VERSION_MINOR}.${ADEPT_VERSION_PATCH})

# Project Info
set(PACKAGE_NAME          "adept")
set(PACKAGE_VERSION       "${ADEPT_VERSION_PATCH}-dev")
set(PACKAGE_VERSION_HEX   "0x01000000")
set(PACKAGE_STRING        "${PACKAGE_NAME} ${PACKAGE_VERSION}")
set(PACKAGE_TARNAME       "${PACKAGE_NAME}-${PACKAGE_VERSION}")
set(PACKAGE_BUGREPORT     "https://github.com/rjhogan/Adept-2")

set(CMAKE_EXPORT_COMPILE_COMMANDS ON CACHE INTERNAL "") # works

# Default build type for single-config generators (set this before project() command)
# For multi-config generators, such as Visual Studio, use: cmake --build . --config=<CONFIG>
if (NOT CMAKE_BUILD_TYPE OR CMAKE_BUILD_TYPE STREQUAL "")
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Build type for single-config generators" FORCE)
endif()
message("-- Build Mode: ${CMAKE_BUILD_TYPE}")
message("-- MSVC_RUNTIME: ${MSVC_RUNTIME}")

project(${PACKAGE_NAME} LANGUAGES CXX DESCRIPTION "The ADEPT C++ Library")

# Project shared libs ON for UNIX
if (NOT DEFINED BUILD_SHARED_LIBS)
    set(BUILD_SHARED_LIBS ${UNIX})
endif()

# Require C++11 or higher
if (NOT DEFINED CMAKE_CXX_STANDARD)
    set(CMAKE_CXX_STANDARD 17)
elseif(CMAKE_CXX_STANDARD LESS 17)
    message(FATAL_ERROR "Please specify CMAKE_CXX_STANDARD of 17 or higher")
endif()

if (NOT DEFINED CMAKE_CXX_STANDARD_REQUIRED)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
endif()
# Avoid use of compiler language extensions, i.e. -std=c++11 not -std=gnu++11
if (NOT DEFINED CMAKE_CXX_EXTENSIONS)
    set(CMAKE_CXX_EXTENSIONS FALSE)
endif()

# If available, use PIC for shared libs and PIE for executables
if (NOT DEFINED CMAKE_POSITION_INDEPENDENT_CODE)
    set(CMAKE_POSITION_INDEPENDENT_CODE ON)
endif()

if (CMAKE_POSITION_INDEPENDENT_CODE)
    # cmake policy CMP0083: add PIE support if possible (need cmake 3.14)
    include(CheckPIESupported)
    check_pie_supported()
endif()

set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})
include(Platform)

message("-- CMAKE_BUILD_TYPE: ${CMAKE_BUILD_TYPE}")
message("-- CMAKE_CXX_FLAGS: ${CMAKE_CXX_FLAGS}")
message("-- CMAKE_CXX_STANDARD: ${CMAKE_CXX_STANDARD}")
message("-- BUILD_SHARED_LIBS: ${BUILD_SHARED_LIBS}")
include_directories(${CMAKE_SOURCE_DIR}/include)
message("-- ADEPT INCLUDE DIR: ${PROJECT_SOURCE_DIR}/include")

add_subdirectory(adept)

