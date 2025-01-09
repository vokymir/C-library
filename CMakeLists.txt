cmake_minimum_required(VERSION 3.10)

project(Clib)

# Set the output path for the build
set(OSBitness 32)
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(OSBitness 64)
endif()
set(FullOutputDir "${CMAKE_SOURCE_DIR}/bin/${CMAKE_SYSTEM_NAME}${OSBitness}/${CMAKE_BUILD_TYPE}")

# Set the output directory for the build
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${FullOutputDir}/static_libs")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${FullOutputDir}")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${FullOutputDir}")

# Include the source directory
include_directories(.)

add_library(lib SHARED src/lib/lib.c)


add_executable(${PROJECT_NAME} src/main.c)

target_link_libraries(${PROJECT_NAME} lib)