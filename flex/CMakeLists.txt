cmake_minimum_required(VERSION 3.0.0 FATAL_ERROR)

set(PROJECT_NAME win_flex)

project(${PROJECT_NAME} C)

file(GLOB src
    "src/*.c"
    "src/*.h"
)

# exclude some source files
list(REMOVE_ITEM src "${CMAKE_CURRENT_SOURCE_DIR}/src/libmain.c")
list(REMOVE_ITEM src "${CMAKE_CURRENT_SOURCE_DIR}/src/libyywrap.c")

# Add executable to build.
add_executable(${PROJECT_NAME} 
   ${src}
)

target_compile_definitions(${PROJECT_NAME}
    PRIVATE "_CONSOLE")

target_include_directories(${PROJECT_NAME} PRIVATE "src")

target_link_libraries(${PROJECT_NAME} 
    PUBLIC winflexbison_common)

# libfl
add_library(fl STATIC 
    "${CMAKE_CURRENT_SOURCE_DIR}/src/libmain.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/libyywrap.c")

target_include_directories(fl
    PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/src/")
