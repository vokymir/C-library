cmake_minimum_required(VERSION 3.10)

project(C_lib VERSION 0.0.1)
set (PROJECT_TYPE SHARED) # options are SHARED or STATIC

#==========BIN & WIN SUPPORT==========
# Set the output path for the build, provides cleaner output
set(OS_BITNESS 32)
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(OS_BITNESS 64)
endif()
set(FULL_OUTPUT_DIR "${CMAKE_SOURCE_DIR}/bin/${CMAKE_SYSTEM_NAME}${OS_BITNESS}/${CMAKE_BUILD_TYPE}")

# Set the output directory for the build
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${FULL_OUTPUT_DIR}/static_libs")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${FULL_OUTPUT_DIR}")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${FULL_OUTPUT_DIR}")

# Ensure windows build
if(MSVC)
    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
endif()

#==========TESTING==========
include(CTest)
enable_testing()

#==========MACROS==========
add_compile_definitions(IFDEFINE) # Define a macro
add_compile_definitions(OTHER_DEFINE=4) # Define a macro with a value

#==========INCLUDES & LIBRARIES==========

# Include the source directory
set(SOURCE_DIR "${CMAKE_SOURCE_DIR}/src")
include_directories(${SOURCE_DIR})

# Set all libraries which should be built
set(LIBRARIES
    lib
)

# Add the main executable
add_executable(${PROJECT_NAME} ${SOURCE_DIR}/main.c)

# Add all libraries
foreach(LIBRARY ${LIBRARIES})
    add_library(${LIBRARY} ${PROJECT_TYPE} ${SOURCE_DIR}/${LIBRARY}/${LIBRARY}.c)
    target_link_libraries(${PROJECT_NAME} ${LIBRARY})
endforeach(LIBRARY ${LIBRARIES})