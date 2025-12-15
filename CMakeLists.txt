cmake_minimum_required(VERSION 3.20)
project(SenpaiAppEngine VERSION 0.1.0 LANGUAGES CXX)

# Set C++23 as the standard
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Set the default build type to Debug if not specified
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the type of build." FORCE)
endif()

# Options to control what gets built
option(SENPAI_BUILD_EXAMPLES "Build example game (FiveKnights)" OFF)
option(SENPAI_BUILD_TESTS "Build tests" OFF)

# add SenpaiEngine
add_subdirectory(./Senpai)

# add Game only if requested
if(SENPAI_BUILD_EXAMPLES)
    add_subdirectory(./Game)
endif()

# GTest - only if building tests
if(SENPAI_BUILD_TESTS)
    include(FetchContent)
    
    FetchContent_Declare(
      googletest
      URL https://github.com/google/googletest/archive/refs/tags/release-1.12.1.zip
    )
    
    set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
    FetchContent_MakeAvailable(googletest)

    add_executable(Tests ./tests/Tests.c++)
    target_link_libraries(Tests PRIVATE gtest_main gtest Senpai)
endif()