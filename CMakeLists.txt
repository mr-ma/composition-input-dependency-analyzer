cmake_minimum_required(VERSION 3.12)

project(input-dependency-analyzer VERSION 0.1 LANGUAGES CXX)

set(JSON_BuildTests OFF)
add_subdirectory(json)

set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DNDEBUG")

add_definitions(-DHAVE_LLVM)
add_definitions(-DENABLE_CFG)

add_subdirectory(Analysis)  # Use your pass name here.
add_subdirectory(Transforms)  # Use your pass name here.
#add_subdirectory(OH)  # Use your pass name here.
#add_subdirectory(CutVertice)  # Use your pass name here.

add_library(input-dependency-analyzer::Transforms ALIAS Transforms)
add_library(input-dependency-analyzer::InputDependency ALIAS InputDependency)