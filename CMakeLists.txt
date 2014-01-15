#            Copyright (c) Dean Michael Berris 2010.
# Distributed under the Boost Software License, Version 1.0.
#    (See accompanying file LICENSE_1_0.txt or copy at
#          http://www.boost.org/LICENSE_1_0.txt)

cmake_minimum_required(VERSION 2.8)
project(CPP-NETLIB)

include("HunterGate.cmake")
include(hunter_add_package)
hunter_add_package(OpenSSL)
hunter_add_package(Sugar)
hunter_add_package(
    Boost
    COMPONENTS
    chrono
    date_time
    filesystem
    program_options
    regex
    system
    test
    thread
)
include("${SUGAR_ROOT}/cmake/Sugar")
if(APPLE)
  hunter_add_package(ios_sim)
endif()

set(Boost_USE_STATIC_LIBS ON)
set(Boost_USE_MULTI_THREADED ON)
find_package( Boost 1.45.0 REQUIRED unit_test_framework system regex date_time thread filesystem program_options chrono )
find_package( OpenSSL )
find_package( Threads )
set(CMAKE_VERBOSE_MAKEFILE true)

include_directories("./") # before dependent includes!

add_definitions(-DBOOST_ALL_NO_LIB)

if (CMAKE_BUILD_TYPE MATCHES Debug)
    add_definitions(-DBOOST_NETWORK_DEBUG)
endif()

if (OPENSSL_FOUND)
    add_definitions(-DBOOST_NETWORK_ENABLE_HTTPS)
    include_directories(${OPENSSL_INCLUDE_DIR})
endif()

if (${CMAKE_CXX_COMPILER_ID} MATCHES GNU)
  set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")
endif()

# http://boost.2283326.n4.nabble.com/config-clang-int128-support-td4653826.html
check_symbol_exists(__SIZEOF_INT128__ "" has_int128)
if(NOT MSVC AND has_int128)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DBOOST_HAS_INT128=1")
endif()

if(NOT MSVC)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -ftemplate-depth=1024")
endif()

if (Boost_FOUND)
    if (MSVC)
      add_definitions(-D_SCL_SECURE_NO_WARNINGS)
    endif(MSVC)
    if (WIN32)
      add_definitions(-D_WIN32_WINNT=0x0501)
    endif(WIN32)
    include_directories(${Boost_INCLUDE_DIRS})
    enable_testing()
    add_subdirectory(libs/network/src)
    add_subdirectory(libs/network/test)
    if (NOT MSVC)
      add_subdirectory(libs/mime/test)
    endif(NOT MSVC)
    add_subdirectory(libs/network/example)
endif(Boost_FOUND)

if (MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /bigobj")
endif()

enable_testing()

install(DIRECTORY boost DESTINATION include)
