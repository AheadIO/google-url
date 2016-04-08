#-------------------------------------------------------------------------------
# Cmake configuration
#-------------------------------------------------------------------------------
cmake_minimum_required(VERSION 2.8.11)

#-------------------------------------------------------------------------------
# project configuration
#-------------------------------------------------------------------------------
set(PROJECT "google-url")
project(${PROJECT})

#-------------------------------------------------------------------------------
# Build Options
#-------------------------------------------------------------------------------
add_compile_options(-fPIC)
add_compile_options(-std=c++11) # c++11
add_compile_options(-fno-rtti)  # Removing rtti

#-------------------------------------------------------------------------------
# Collect source
#-------------------------------------------------------------------------------
file(GLOB_RECURSE BASE_SRC_FILES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} base/*.cc)
file(GLOB_RECURSE BASE_HDR_FILES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} base/*.h)

file(GLOB_RECURSE SRC_FILES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} src/*.cc)
file(GLOB_RECURSE HDR_FILES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} src/*.h)
list(REMOVE_ITEM BASE_SRC_FILES "base/logging.cc")
list(REMOVE_ITEM BASE_HDR_FILES "base/logging.h")

#-------------------------------------------------------------------------------
# Software Dependencies
#-------------------------------------------------------------------------------
find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

#-------------------------------------------------------------------------------
# Create binaries
#-------------------------------------------------------------------------------
set(include_dirs
    ${CMAKE_CURRENT_SOURCE_DIR}/..
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/third_party
    )

include_directories(${include_dirs})

if(CMAKE_SHARED_LIBS)
    set(LIB_TYPE "SHARED")
else()
    set(LIB_TYPE "STATIC")
endif()

add_library(${PROJECT} ${LIB_TYPE}
	${SRC_FILES}
	${SRC_FILES}
	${BASE_SRC_FILES}
	${BASE_SRC_FILES}
    )

target_link_libraries(
    ${PROJECT}
    ${GTEST_BOTH_LIBRARIES}
    )


#-------------------------------------------------------------------------------
# Install
#-------------------------------------------------------------------------------
install(DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/src"
        DESTINATION "include/googleurl"
        FILES_MATCHING PATTERN "*.h"
)

install(DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/base"
        DESTINATION "include/googleurl"
        FILES_MATCHING PATTERN "*.h"
)

install(DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/third_party/jml"
        DESTINATION "include"
        FILES_MATCHING PATTERN "*.h"
)

install( TARGETS ${PROJECT}
    COMPONENT Runtime
    RUNTIME DESTINATION "bin"
    LIBRARY DESTINATION "bin"
    ARCHIVE DESTINATION "lib"
)


