cmake_minimum_required(VERSION 3.3)
project(rf_msgs)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++14 -g)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  message_generation
)

add_message_files(
  FILES
  Wifi.msg
  Bearing.msg
  AccessPoints.msg
  Station.msg
)
generate_messages(
  DEPENDENCIES std_msgs rf_msgs
)

catkin_package(
  CATKIN_DEPENDS std_msgs roscpp
)
