cmake_minimum_required(VERSION 3.18 FATAL_ERROR)
cmake_policy(VERSION 3.18.0)

project (Blossom5
    VERSION 2.05.1 # .1 for cgg changes
    LANGUAGES C CXX
    )

include(FetchContent)
FetchContent_Declare(blossom5
    URL https://pub.ista.ac.at/~vnk/software/blossom5-v2.05.src.tar.gz
    URL_HASH SHA224=873dca80593f810d996c1bf4531625b2a0c18e81ea0a4a7fdf326a93
    DOWNLOAD_EXTRACT_TIMESTAMP TRUE
    SOURCE_DIR "src/blossom5" # required for #include <blossom5/...>
    PATCH_COMMAND git apply --ignore-space-change --ignore-whitespace -p1 "${CMAKE_CURRENT_SOURCE_DIR}/patches/blossom.patch"
    )
FetchContent_MakeAvailable(blossom5)

add_library(Blossom5 
    ${blossom5_SOURCE_DIR}/PMinterface.cpp
    ${blossom5_SOURCE_DIR}/PMexpand.cpp
    ${blossom5_SOURCE_DIR}/MinCost/MinCost.cpp
    #${blossom5_SOURCE_DIR}/GEOM/GPMinterface.cpp
    #${blossom5_SOURCE_DIR}/GEOM/GPMkdtree.cpp
    #${blossom5_SOURCE_DIR}/GEOM/GPMinit.cpp
    #${blossom5_SOURCE_DIR}/GEOM/GPMmain.cpp
    ${blossom5_SOURCE_DIR}/PMshrink.cpp
    ${blossom5_SOURCE_DIR}/PMinit.cpp
    ${blossom5_SOURCE_DIR}/PMmain.cpp
    ${blossom5_SOURCE_DIR}/PMrepair.cpp
    ${blossom5_SOURCE_DIR}/PMduals.cpp
    ${blossom5_SOURCE_DIR}/misc.cpp
    )
set_property(TARGET Blossom5 PROPERTY POSITION_INDEPENDENT_CODE ON)

option(BUILD_SHARED_LIBS "Build as shared instead of static library")

target_include_directories(Blossom5
  PUBLIC "${blossom5_SOURCE_DIR}/.."
  #$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src/blossom5>
  #     $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/>
  #      $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

add_library(Blossom5::Blossom5 ALIAS Blossom5)

# Disabled for now, needs to build with GEOM:

#if(${CMAKE_PROJECT_NAME} STREQUAL "${PROJECT_NAME}")
#    add_executable(blossom5 ${blossom5_SOURCE_DIR}/example.cpp)
#    target_link_libraries(blossom5 PRIVATE Blossom5::Blossom5)
#endif()
