cmake_minimum_required(VERSION 3.22.1)
project(PointCloudDescriptors)

include(ExternalProject)


set(CMAKE_CXX_STANDARD 20)

ExternalProject_add(qhull
        GIT_REPOSITORY https://github.com/qhull/qhull.git
        GIT_TAG v7.3.1
        CMAKE_ARGS "-DCMAKE_INSTALL_PREFIX=${PROJECT_BINARY_DIR}/thirdparty-install;-DCMAKE_POSITION_INDEPENDENT_CODE=ON"
        PREFIX qhull-prefix
)

cmake_policy(SET CMP0144 NEW) # set CMP0144 to NEW to enable if() generator expressions

find_package(PCL 1.3 REQUIRED)
cmake_policy(SET CMP0167 NEW) # set CMP0167 to NEW to enable if() generator expressions
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

add_executable(PointCloudDescriptors main.cpp
        PointCloudDescriptor.h
        PointCloudDescriptor.cpp
        cnpy_header_only.h)

target_link_libraries(PointCloudDescriptors ${PCL_LIBRARIES})
