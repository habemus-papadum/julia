project(Julia C CXX)

cmake_minimum_required(VERSION 3.5)

## Purpose : a create a skeleton CMakeLists file for julia, so that Clion can be used to help grok the code base
## Non Goal: (at least initially) Create a Cmake build file that actually can build anything

## The focus is only on creating a mechanism to facilitate grokking with Clion

## Prereq:  build julia with `make` or `make O=<dir>>
set(JULIA_BOOTSTRAP_BUILD_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} CACHE PATH "Build directory in which julia was built using the Makefile system")

set(JULIA_LLVM_VERSION 3.7.1.)
set(LLVM_CONFIG ${JULIA_BOOTSTRAP_BUILD_DIRECTORY}/usr/tools/llvm-config)

execute_process(COMMAND "${LLVM_CONFIG}" --cxxflags
        OUTPUT_STRIP_TRAILING_WHITESPACE
        OUTPUT_VARIABLE LLVM_CXXFLAGS)

FILE(GLOB_RECURSE SOURCES src/*.cpp src/*.c src/*.h src/*.hpp)
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${LLVM_CONFIG}")
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
add_definitions(-DLLVM_SHLIB)

include_directories(src src/flisp src/support ${JULIA_BOOTSTRAP_BUILD_DIRECTORY}/usr/include)

add_library(julialib ${SOURCES})