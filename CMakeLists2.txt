cmake_minimum_required(VERSION 3.3)
project(rf_msgs)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++14 -g)

find_package(ament_cmake REQUIRED)
find_package(std_msgs REQUIRED)
find_package(rosidl_default_generators REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  msg/Wifi.msg
  msg/Bearing.msg
  msg/AccessPoints.msg
  msg/Station.msg
  DEPENDENCIES std_msgs
)

ament_export_dependencies(std_msgs rclcpp rosidl_default_runtime)
ament_package()
