cmake_minimum_required(VERSION 3.3)
project(rf_msgs)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++14 -g)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
if(TARGET_ROS2)
  message("Targeting ROS2")
  find_package(ament_cmake REQUIRED)
  find_package(std_msgs REQUIRED)
  find_package(rosidl_default_generators REQUIRED)
else()
  find_package(catkin REQUIRED COMPONENTS
    roscpp
    std_msgs
    message_generation
  )
endif()

if(TARGET_ROS2)
  rosidl_generate_interfaces(${PROJECT_NAME}
    msg/Wifi.msg
    msg/Bearing.msg
    msg/AccessPoints.msg
    msg/Station.msg
    DEPENDENCIES std_msgs
  )

else()
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
endif()

if(TARGET_ROS2)
  ament_export_dependencies(std_msgs roscpp rosidl_default_runtime)
  ament_package()
else()
  catkin_package(
    CATKIN_DEPENDS std_msgs roscpp
  )
endif()
