cmake_minimum_required(VERSION 3.0.0 FATAL_ERROR)

set(PROJECT_NAME win_bison)

project(${PROJECT_NAME} C)

file(GLOB bison_src
    "${CMAKE_CURRENT_SOURCE_DIR}/src/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/*.h"
)

# Add executable to build.
add_executable(${PROJECT_NAME} ${bison_src})

add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory "${CMAKE_CURRENT_SOURCE_DIR}/data" "$<TARGET_FILE_DIR:${PROJECT_NAME}>/data"
)

target_compile_definitions(${PROJECT_NAME}
    PRIVATE "_CONSOLE")

target_include_directories(${PROJECT_NAME} 
    PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/src"
    PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}")

target_link_libraries(${PROJECT_NAME} 
    PRIVATE winflexbison_common)

# liby
file(GLOB liby_src
    "lib/*.c"
    "lib/*.h"
)

add_library(y STATIC ${liby_src})

target_include_directories(y
    PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/src/")