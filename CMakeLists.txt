cmake_minimum_required(VERSION 4.2)

project(SDL_nmix C)

find_package(SDL2 CONFIG REQUIRED)

# SDL_sound
include(FetchContent)
FetchContent_Declare(
  SDL_sound
  GIT_REPOSITORY https://github.com/hghpublic/icculus-SDL_sound
  GIT_TAG        SDL_sound/2.0.3/gltron
)
FetchContent_MakeAvailable(SDL_sound)

set(target SDL_nmix)
add_library(${target} STATIC
    SDL_nmix.c
    SDL_nmix_file.c
)
target_include_directories(${target} INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})
target_compile_features(${target} PRIVATE c_std_23)
target_link_libraries(${target} PRIVATE SDL2::SDL2)
target_link_libraries(${target} PUBLIC SDL2_sound)

# examples

if(PROJECT_IS_TOP_LEVEL)
  add_subdirectory(examples)
endif(PROJECT_IS_TOP_LEVEL)