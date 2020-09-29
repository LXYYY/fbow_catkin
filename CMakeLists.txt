cmake_minimum_required(VERSION 2.8.3)
project(fbow_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

SET(FBOW_SRC_PATH "fbow_src")
SET(FBOW_BUILD_PATH "fbow_build")
ExternalProject_Add(
  fbow_src
  GIT_REPOSITORY https://github.com/rmsalinas/fbow.git
  GIT_TAG e148dbc
  UPDATE_COMMAND ""
  SOURCE_DIR ${FBOW_SRC_PATH}
  BINARY_DIR ${FBOW_BUILD_PATH}
  CONFIGURE_COMMAND cd ../${FBOW_BUILD_PATH} && cmake 
    ../${FBOW_SRC_PATH} -DCMAKE_INSTALL_PREFIX=${CATKIN_DEVEL_PREFIX}
  BUILD_COMMAND cd ../${FBOW_BUILD_PATH} && make -j8
  INSTALL_COMMAND cd ../${FBOW_BUILD_PATH} && make install -j8
)

cs_install()
cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
          LIBRARIES fbow)

