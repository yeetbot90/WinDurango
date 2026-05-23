cmake_minimum_required(VERSION 4.0)
project(WinDurango.KernelX VERSION 1.0.0)

set(VERSION_SUFFIX "-dev.1") # used for non-stable versions, otherwise blank
set(CMAKE_CXX_STANDARD 20)

set(FILES
    src/kernelx.cpp
    src/dllmain.cpp
    src/Hooks.cpp
    src/CurrentApp.cpp
    include/WinDurango.KernelX/CurrentApp.h
    include/WinDurango.KernelX/kernelx.h
    include/WinDurango.KernelX/Logan.h
    include/WinDurango.KernelX/Hooks.h
)

add_library(WinDurango.KernelX SHARED ${FILES})

find_path(DETOURS_INCLUDE_DIR detours.h)
find_library(DETOURS_LIBRARY detours)

if(NOT DETOURS_INCLUDE_DIR OR NOT DETOURS_LIBRARY)
    message(FATAL_ERROR "detours was not found via vcpkg/toolchain. Make sure dependencies are installed for the selected VCPKG_TARGET_TRIPLET.")
endif()

target_link_libraries(WinDurango.KernelX PRIVATE 
    WinDurango.Common 
    WinDurango.Implementation.WinRT 
    ${DETOURS_LIBRARY}
)

target_link_options(WinDurango.KernelX
    PUBLIC 
    $<$<CXX_COMPILER_ID:MSVC>:/FORCE:MULTIPLE>
)

target_include_directories(WinDurango.KernelX PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/include/WinDurango.KernelX/
    ${CMAKE_SOURCE_DIR}/projects/WinDurango.Common/include
    ${CMAKE_SOURCE_DIR}/projects/WinDurango.Implementation.WinRT/include
    ${DETOURS_INCLUDE_DIR}
)

set_target_properties(WinDurango.KernelX PROPERTIES
    OUTPUT_NAME "kernelx"
)