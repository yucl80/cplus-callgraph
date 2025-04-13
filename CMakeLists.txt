cmake_minimum_required(VERSION 3.10)
project(CppCallGraphAnalyzer)

set(CMAKE_CXX_STANDARD 17)

find_package(LLVM REQUIRED CONFIG)
find_package(Clang REQUIRED CONFIG)

message(STATUS "Found LLVM ${LLVM_PACKAGE_VERSION}")
message(STATUS "Using LLVMConfig.cmake in: ${LLVM_DIR}")
message(STATUS "Using ClangConfig.cmake in: ${Clang_DIR}")

include_directories(${LLVM_INCLUDE_DIRS})
include_directories(${CLANG_INCLUDE_DIRS})

# Use LLVM's recommended way to link components
llvm_map_components_to_libnames(llvm_libs
    support
    core
    option
)

add_executable(callgraph_analyzer main.cpp)

# Link against libclang.so.1 with explicit path
link_directories(/usr/lib/llvm-18/lib)
target_link_libraries(callgraph_analyzer
    ${llvm_libs}
    clang-18
    LLVMSupport
)
