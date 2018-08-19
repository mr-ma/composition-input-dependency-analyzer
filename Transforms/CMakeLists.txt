cmake_minimum_required(VERSION 3.12)

add_library(Transforms SHARED
        FunctionClone.h
        FunctionClonePass.h
        FunctionExtraction.h
        FunctionSnippet.h
        Utils.h

        FunctionClonePass.cpp
        FunctionClone.cpp
        FunctionExtraction.cpp
        FunctionSnippet.cpp
        Utils.cpp
        )

find_package(LLVM 6.0 REQUIRED CONFIG)
target_include_directories(Transforms
        PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/>
        $<INSTALL_INTERFACE:include>
        PRIVATE
        ${LLVM_INCLUDE_DIRS})

if ($ENV{CLION_IDE})
    include_directories("/usr/include/llvm-6.0/")
    include_directories("/usr/include/llvm-c-6.0/")
endif ()

target_link_libraries(Transforms PRIVATE input-dependency-analyzer::InputDependency)

target_compile_features(Transforms PRIVATE cxx_std_17)

target_compile_options(Transforms PRIVATE -fno-rtti)

install(DIRECTORY ./ DESTINATION /usr/local/include/input-dependency
        FILES_MATCHING PATTERN "*.h")
install(TARGETS Transforms LIBRARY DESTINATION /usr/local/lib)


# Get proper shared-library behavior (where symbols are not necessarily
# resolved when the shared library is linked) on OS X.
if (APPLE)
    set_target_properties(Transforms PROPERTIES
            LINK_FLAGS "-undefined dynamic_lookup"
            )
endif (APPLE)

