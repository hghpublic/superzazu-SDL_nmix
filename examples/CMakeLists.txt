# this file is just a helper to build examples and performance tests; if you
# want to use the library just drop the SDL_nmix.* files into your project.

cmake_minimum_required(VERSION 4.2 FATAL_ERROR)
project(SDL_nmix C)

# set(CMAKE_C_STANDARD 99)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake")

if(MSVC)
    add_compile_options(/W4 /WX)
else()
    add_compile_options(-Wall -Wextra -pedantic)
endif()

# examples

set(examples
    1_basic
    2_file
    3_stresstest
    4_demo
    5_stresstest_decoding
)

foreach(target IN LISTS examples)
    add_executable(${target} ${target}.c)
    target_compile_features(${target} PRIVATE c_std_23)
    target_link_libraries(${target} PRIVATE SDL_nmix)
    target_link_libraries(${target} PRIVATE SDL2::SDL2)    
    # link to libm on non-windows builds:
    if(NOT WIN32)
        target_link_libraries(${target} PRIVATE m)
    endif()
    set_target_properties(${target} PROPERTIES FOLDER examples)
endforeach()


# tests

# performance tests
set(target test_nmix)
add_executable(${target} ${target}.c)
target_compile_features(${target} PRIVATE c_std_23)
target_link_libraries(${target} PRIVATE SDL_nmix)
target_link_libraries(${target} PRIVATE SDL2::SDL2)    
# link to libm on non-windows builds:
if(NOT WIN32)
    target_link_libraries(${target} PRIVATE m)
endif()
set_target_properties(${target} PROPERTIES FOLDER tests)

option(BUILD_SDLMIXER_TEST "Build SDL_mixer test program" ON)
message(STATUS "Build SDL_mixer test program: ${BUILD_SDLMIXER_TEST}")
if(BUILD_SDLMIXER_TEST)
    set(target test_sdlmixer)
    add_executable(${target} test_sdlmixer.c)
    target_compile_features(${target} PRIVATE c_std_23)
    find_package(SDL2_mixer CONFIG REQUIRED)
    target_link_libraries(${target} PRIVATE SDL2::SDL2 SDL2_mixer::SDL2_mixer)
    if(NOT WIN32)
        target_link_libraries(${target} PRIVATE m)
    endif()
    set_target_properties(${target} PROPERTIES FOLDER tests)
endif()
    
