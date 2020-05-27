################################################################################
# Created by mspieler on 27.5.2020.
# Modified CMakeLists.txt to create a catkin package

cmake_minimum_required(VERSION 3.0)
project(mpeg-pcc-tmc13)

# Cpp standard version
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

enable_language(C)

find_package(catkin REQUIRED COMPONENTS
)

catkin_package(
  INCLUDE_DIRS
    tmc3
  LIBRARIES
    ${PROJECT_NAME}
  CATKIN_DEPENDS
  DEPENDS
)


# config file
include(CheckSymbolExists)
check_symbol_exists(getrusage sys/resource.h HAVE_GETRUSAGE)

configure_file (
  ${CMAKE_CURRENT_SOURCE_DIR}/tmc3/TMC3Config.h.in
  ${PROJECT_BINARY_DIR}/tmc3/TMC3Config.h
)

#########################
# Build static library ##
#########################

add_library(${PROJECT_NAME} STATIC
  tmc3/AttributeCommon.cpp
  tmc3/AttributeDecoder.cpp
  tmc3/AttributeEncoder.cpp
  tmc3/DualLutCoder.cpp
  tmc3/FixedPoint.cpp
  tmc3/OctreeNeighMap.cpp
  tmc3/RAHT.cpp
  tmc3/decoder.cpp
  tmc3/encoder.cpp
  tmc3/entropydirac.cpp
  tmc3/geometry_intra_pred.cpp
  tmc3/geometry_octree.cpp
  tmc3/geometry_octree_decoder.cpp
  tmc3/geometry_octree_encoder.cpp
  tmc3/geometry_trisoup_decoder.cpp
  tmc3/geometry_trisoup_encoder.cpp
  tmc3/io_hls.cpp
  tmc3/io_tlv.cpp
  tmc3/misc.cpp
  tmc3/osspecific.cpp
  tmc3/partitioning.cpp
  tmc3/pcc_chrono.cpp
  tmc3/ply.cpp
  tmc3/pointset_processing.cpp
  tmc3/quantization.cpp
  tmc3/tables.cpp
  dependencies/arithmetic-coding/src/ArithmeticCodec.cpp
  dependencies/schroedinger/schroarith.c
)
target_include_directories(${PROJECT_NAME} PUBLIC
  ${CMAKE_CURRENT_SOURCE_DIR}
  tmc3
  dependencies/nanoflann
  dependencies/arithmetic-coding/inc
  ${PROJECT_BINARY_DIR}/tmc3
)

############
# Install ##
############

install(
  TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY tmc3
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})
