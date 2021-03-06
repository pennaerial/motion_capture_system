cmake_minimum_required(VERSION 2.8.3)
project(mocap_base)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_LIST_DIR}/cmake")

find_package(Boost REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(catkin REQUIRED COMPONENTS
  roscpp
  geometry_msgs
  eigen_conversions
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES mocap_base_driver mocap_kalman_filter
  CATKIN_DEPENDS
    geometry_msgs
  DEPENDS
  Boost Eigen3
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIR}
)

add_definitions("-std=c++0x -Wall")

add_library(mocap_kalman_filter
  src/KalmanFilter.cpp
)
target_link_libraries(mocap_kalman_filter
  ${catkin_LIBRARIES}
)
add_dependencies(mocap_kalman_filter
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)

add_library(mocap_base_driver
  src/MoCapDriverBase.cpp
)
target_link_libraries(mocap_base_driver
  ${catkin_LIBRARIES}
  mocap_kalman_filter
)
add_dependencies(mocap_base_driver
  ${${PROJECT_NAME}_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)

