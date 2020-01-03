cmake_minimum_required(VERSION 3.11)

project(swift-demangle)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 14)

set(LLVM_ON_UNIX 1)
#set(HAVE_GETPAGESIZE 1)
set(HAVE_SYSCONF 1)

set(USER_COMPILE_FLAGS "")
string(APPEND CMAKE_C_FLAGS   " ${USER_COMPILE_FLAGS}")
string(APPEND CMAKE_CXX_FLAGS " ${USER_COMPILE_FLAGS}")


set (LLVM_HOST_TRIPLE "X86_64-pc-windows-msvc")
set (PACKAGE_NAME "WTF")
set (PACKAGE_VERSION "1024.768")

include_directories(
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/swift/include/
        ${CMAKE_CURRENT_SOURCE_DIR}/llvm/llvm/include/
		${CMAKE_CURRENT_SOURCE_DIR}/swift/include/swift/Runtime/
        )

FILE(GLOB SWIFT_SOURCE_FILES
        "swift/lib/Demangling/*.cpp"
)


FILE(GLOB LLVM_SOURCE_FILES
        "llvm/llvm/lib/Support/*.c*"
        "llvm/llvm/lib/Demangle/*.cpp"
)

FILE(GLOB DEMANGLE_SOURCE_FILES
        swift-demangle.cpp
)

configure_file("llvm/llvm/include/llvm/Config/abi-breaking.h.cmake"  "${CMAKE_CURRENT_SOURCE_DIR}/llvm/llvm/include/llvm/Config/abi-breaking.h")
configure_file("llvm/llvm/include/llvm/Config/config.h.cmake"  "${CMAKE_CURRENT_SOURCE_DIR}/llvm/llvm/include/llvm/Config/config.h")
configure_file("llvm/llvm/include/llvm/Config/llvm-config.h.cmake"  "${CMAKE_CURRENT_SOURCE_DIR}/llvm/llvm/include/llvm/Config/llvm-config.h")
configure_file("swift/include/swift/Runtime/CMakeConfig.h.in"  "${CMAKE_CURRENT_SOURCE_DIR}/swift/include/swift/Runtime/CMakeConfig.h")

if ( ${CMAKE_SIZEOF_VOID_P} EQUAL 8 )
add_executable(swift-demangle
    "${SWIFT_SOURCE_FILES}"
    "${LLVM_SOURCE_FILES}"
    "${DEMANGLE_SOURCE_FILES}"
    )
else()
add_executable(swift-demangle
    "${SWIFT_SOURCE_FILES}"
    "${LLVM_SOURCE_FILES}"
    "${DEMANGLE_SOURCE_FILES}"
    )
endif()
