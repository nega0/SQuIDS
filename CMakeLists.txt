cmake_minimum_required(VERSION 2.8)
project(SQuIDS)

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules)

set(CMAKE_C_FLAGS "-O3 -fPIC")
set(CMAKE_CXX_FLAGS "-std=c++11 ${CMAKE_C_FLAGS}")

find_package(GSL 1.14 REQUIRED QUIET)
## this version of FindGSL.cmake doesn't support the REQUIRED flag
if(NOT GSL_FOUND)
  message(FATAL_ERROR "gsl (>= 1.14) not found")
endif()
#foreach(v GSL_FOUND GSL_INCLUDE_DIR GSL_LIBRARY GSL_CBLAS_LIBRARY GSL_LIBRARIES GSL_LIBRARY_DIR GSL_CFLAGS)
#message(STATUS "${v} = ${${v}}")
#endforeach()

file(GLOB SRCS ${CMAKE_SOURCE_DIR}/src/*.cpp)
message(STATUS "csd: ${SRCS}")
include_directories(${CMAKE_SOURCE_DIR}/inc ${CMAKE_SOURCE_DIR}/inc/SU_inc)
set_source_files_properties(${CMAKE_SOURCE_DIR}/src/SUNalg.cpp COMPILE_FLAGS "-Warray-bounds")
add_library(SQuIDS SHARED ${SRCS})
target_link_libraries(SQuIDS ${GSL_LIBRARIES})
