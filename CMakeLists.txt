project(Julia C CXX)

cmake_minimum_required(VERSION 3.5)

## Purpose : a create a skeleton CMakeLists file for julia, so that Clion can be used to help grok the code base
## Non Goal: (at least initially) Create a Cmake build file that actually can build anything

## The focus is only on creating a mechanism to facilitate grokking with Clion

## Prereq:  build julia with  `make O=../builds/default`>
set(JULIA_BOOTSTRAP_BUILD_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../builds/default)
set(LLVM_CONFIG ${JULIA_BOOTSTRAP_BUILD_DIRECTORY}/usr/tools/llvm-config)

execute_process(COMMAND "${LLVM_CONFIG}" --cxxflags
        OUTPUT_STRIP_TRAILING_WHITESPACE
        OUTPUT_VARIABLE LLVM_CXXFLAGS)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${LLVM_CXXFLAGS}")
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
add_definitions(-DLLVM_SHLIB)
add_definitions(ENABLE_TIMINGS)
add_definitions(TRACE_INFERENCE)

include_directories(src src/flisp src/support ${JULIA_BOOTSTRAP_BUILD_DIRECTORY}/usr/include)

FILE(GLOB SOURCES src/*.cpp src/*.c src/*.h src/*.hpp)
add_library(julialib ${SOURCES})
FILE(GLOB UI_SOURCES ui/*.cpp ui/*.c ui/*.h ui/*.hpp)
add_library(ui ${UI_SOURCES})
FILE(GLOB FLISP_SOURCES src/flisp/*.cpp src/flisp/*.c src/flisp/*.h src/flisp/*.hpp)
add_library(flisp ${FLISP_SOURCES})
FILE(GLOB SUPPORT_SOURCES src/support/*.cpp src/support/*.c src/support/*.h src/support/*.hpp)
add_library(support ${SUPPORT_SOURCES})
