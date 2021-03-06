cmake_minimum_required(VERSION 2.6)

project (vertigo)

# we have our own find script for g2o
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake-modules/")

# check if we have gtsam and / or g2o
find_package(GTSAM)
find_package(G2O)

# set compiler options depending on build type
SET(CMAKE_BUILD_TYPE Release)
if("${CMAKE_BUILD_TYPE}" STREQUAL "")
  set(CMAKE_BUILD_TYPE Release)
endif()

add_definitions(" -std=c++11 ")

set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -mfpmath=sse -msse3 -fPIC -O3 -DNDEBUG")
set(CMAKE_LD_FLAGS_RELEASE "${CMAKE_LD_FLAGS_RELEASE} -O3")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -fPIC -ggdb")

set(PKG_CONFIG_EXPORT_LIBS "-lvertigo-g2o -lvertigo-gtsam")

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/vertigo.pc.in
		${CMAKE_CURRENT_BINARY_DIR}/vertigo.pc @ONLY)

install(FILES ${CMAKE_BINARY_DIR}/vertigo.pc DESTINATION lib/pkgconfig)

add_subdirectory(src)
# example commented out because of obsolete header used in gtsam
# add_subdirectory(examples)
