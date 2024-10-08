cmake_minimum_required(VERSION 3.5)
project(droplet_impl)

option(TEST "whether to test by using conformance test" ON)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR})

if(DEFINED ENV{DIOPI_DROPLET_DIR})
  set(DIOPI_DROPLET_DIR $ENV{DIOPI_DROPLET_DIR})
else()
  set(DIOPI_DROPLET_DIR /usr/local/ptpu)
endif()

find_library(DIOPI_DROPLET_LIB NAMES ${DEVICEIMPL} HINTS ${DIOPI_DROPLET_DIR}/lib)
message(STATUS "DIOPI-IMPL lib: ${DIOPI_DROPLET_LIB}")
if(NOT DIOPI_DROPLET_LIB)
  message(FATAL_ERROR "${DEVICEIMPL} library not found !")
endif()

add_library(${DEVICEIMPL} SHARED IMPORTED GLOBAL)
set_target_properties(${DEVICEIMPL} PROPERTIES IMPORTED_LOCATION ${DIOPI_DROPLET_LIB})

if (TEST)
  add_subdirectory(test)
endif()
