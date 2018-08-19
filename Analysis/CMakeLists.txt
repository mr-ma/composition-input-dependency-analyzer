cmake_minimum_required(VERSION 3.12)

add_library(InputDependency SHARED
        BasicBlockAnalysisResult.h
        BasicBlocksUtils.h
        CachedFunctionAnalysisResult.h
        CachedInputDependencyAnalysis.h
        CFGTraversalPath.h
        CLibraryInfo.h
        ClonedFunctionAnalysisResult.h
        constants.h
        definitions.h
        DependencyAnaliser.h
        DependencyAnalysisResult.h
        DependencyInfo.h
        dot_interfaces.h
        DotPrinter.h
        exception.h
        FunctionAnaliser.h
        FunctionCallDepInfo.h
        FunctionDominanceTree.h
        FunctionDOTGraphPrinter.h
        FunctionInputDependencyResultInterface.h
        IndirectCallSitesAnalysis.h
        InputDepConfig.h
        InputDependencyAnalysis.h
        InputDependencyAnalysisInterface.h
        InputDependencyDebugInfoPrinter.h
        InputDependencyStatistics.h
        InputDependentBasicBlockAnaliser.h
        InputDependentFunctionAnalysisResult.h
        InputDependentFunctions.h
        InputDepInstructionsRecorder.h
        LibFunctionInfo.h
        LibraryInfoCollector.h
        LibraryInfoFromConfigFile.h
        LibraryInfoManager.h
        LLVMIntrinsicsInfo.h
        LoggingUtils.h
        LoopAnalysisResult.h
        LoopTraversalPath.h
        NonDeterministicBasicBlockAnaliser.h
        ReflectingBasicBlockAnaliser.h
        ReflectingDependencyAnaliser.h
        SnakeLibraryInfo.h
        Statistics.h
        STLStringInfo.h
        TransparentCachingPass.h
        Utils.h
        value_dependence_graph.h
        ValueDepInfo.h

        BasicBlockAnalysisResult.cpp
        CLibraryInfo.cpp
        DependencyAnaliser.cpp
        FunctionAnaliser.cpp
        CachedFunctionAnalysisResult.cpp
        ClonedFunctionAnalysisResult.cpp
        FunctionCallDepInfo.cpp
        FunctionDOTGraphPrinter.cpp
        IndirectCallSitesAnalysis.cpp
        InputDependencyAnalysisPass.cpp
        InputDependencyAnalysis.cpp
        CachedInputDependencyAnalysis.cpp
        InputDependencyDebugInfoPrinter.cpp
        InputDependencyStatistics.cpp
        #InputDependentBasicBlockAnaliser.cpp
        LibFunctionInfo.cpp
        LibraryFunctionsDebugPass.cpp
        LibraryInfoCollector.cpp
        LibraryInfoManager.cpp
        LoopAnalysisResult.cpp
        ModuleSizeDebugPass.cpp
        NonDeterministicBasicBlockAnaliser.cpp
        NonDeterministicReflectingBasicBlockAnaliser.cpp
        ReflectingBasicBlockAnaliser.cpp
        STLStringInfo.cpp
        LoggingUtils.cpp
        Utils.cpp
        InputDepInstructionsRecorder.cpp
        DotPrinter.cpp
        value_dependence_graph.cpp
        FunctionDominanceTree.cpp
        ValueDepInfo.cpp
        LoopTraversalPath.cpp
        CFGTraversalPath.cpp
        LLVMIntrinsicsInfo.cpp
        BasicBlocksUtils.cpp
        LibraryInfoFromConfigFile.cpp
        Statistics.cpp
        constants.cpp
        TransparentCachingPass.cpp
        )

find_package(LLVM 6.0 REQUIRED CONFIG)
target_include_directories(InputDependency
        PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/>
        $<INSTALL_INTERFACE:include>
        PRIVATE
        ${LLVM_INCLUDE_DIRS})

if ($ENV{CLION_IDE})
    include_directories("/usr/include/llvm-6.0/")
    include_directories("/usr/include/llvm-c-6.0/")
endif ()

target_compile_features(InputDependency PRIVATE cxx_std_17)

target_compile_options(InputDependency PRIVATE -fno-rtti)

target_link_libraries(InputDependency PRIVATE nlohmann_json::nlohmann_json)

install(DIRECTORY ./ DESTINATION /usr/local/include/input-dependency
        FILES_MATCHING PATTERN "*.h")
install(TARGETS InputDependency LIBRARY DESTINATION /usr/local/lib)

# Get proper shared-library behavior (where symbols are not necessarily
# resolved when the shared library is linked) on OS X.
if (APPLE)
    set_target_properties(InputDependency PROPERTIES
            LINK_FLAGS "-undefined dynamic_lookup"
            )
endif (APPLE)
