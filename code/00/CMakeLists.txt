cmake_minimum_required(VERSION 3.0.0)
project(Transform VERSION 0.1.0)

include(CTest)
enable_testing()

#find_package(Eigen3 REQUIRED)
#include_directories("C:/Program Files (x86)/Eigen3/include/eigen3")
#set(Eigen3_DIR CMAKE_INSTALL_PREFIX/share/eigen3/cmake)
find_package(Eigen3 REQUIRED)
include_directories(${PACKAGE_PREFIX_DIR}/include/eigen3)
message(${EIGEN3_INCLUDE_DIR})
add_executable(Transform main.cpp)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
