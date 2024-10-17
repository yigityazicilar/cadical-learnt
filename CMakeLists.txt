cmake_minimum_required(VERSION 3.8)

# set the project name
project("cadical-learnt")

# specify the C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

include(ExternalProject)

# Add the external project
ExternalProject_Add(
    cadical_lib
    SOURCE_DIR ${CMAKE_SOURCE_DIR}/cadical
    CONFIGURE_COMMAND cd ${CMAKE_SOURCE_DIR}/cadical && ./configure
    BUILD_COMMAND cd ${CMAKE_SOURCE_DIR}/cadical && make
    INSTALL_COMMAND ""
    BUILD_BYPRODUCTS ${CMAKE_SOURCE_DIR}/cadical/build/libcadical.a
)

# Add the cadical library as an imported library (use known output path)
add_library(cadical STATIC IMPORTED)
set_target_properties(cadical PROPERTIES IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/cadical/build/libcadical.a)

# Ensure cadical-learnt depends on cadical_lib being built first
add_dependencies(cadical cadical_lib)

# add the executable with source files from the src directory
add_executable(cadical-learnt src/Main.cpp)

# Link the imported cadical library
target_link_libraries(cadical-learnt PRIVATE cadical)

# Include the cadical source directory for header files
target_include_directories(
    cadical-learnt PRIVATE ${CMAKE_SOURCE_DIR}/cadical/src
)