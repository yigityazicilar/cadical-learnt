cmake_minimum_required(VERSION 3.8)

# set the project name
project("cadical-learnt")

# specify the C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_custom_command(
    OUTPUT ${CMAKE_SOURCE_DIR}/cadical/configured
    COMMAND ./configure
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/cadical
)

add_custom_target(
    cadical ALL
    COMMAND make all
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/cadical
    DEPENDS ${CMAKE_SOURCE_DIR}/cadical/configured
)

# add the executable with source files from the src directory
add_executable(cadical-learnt src/main.cpp)

add_dependencies(
    cadical-learnt cadical
)
target_link_libraries(
    cadical-learnt ${CMAKE_SOURCE_DIR}/cadical/build/libcadical.a
)
target_include_directories(
    cadical-learnt PRIVATE ${CMAKE_SOURCE_DIR}/cadical/src
)
