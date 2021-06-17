cmake_minimum_required(VERSION 3.7.2)
project(fetch_gazebo)

find_package(ament_cmake REQUIRED)
find_package(angles REQUIRED)
find_package(control_toolbox REQUIRED)
find_package(gazebo_dev REQUIRED)
find_package(gazebo_ros REQUIRED)
find_package(robot_controllers REQUIRED)
find_package(robot_controllers_interface REQUIRED)
find_package(rclcpp REQUIRED)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

include_directories(include)
link_directories(
  ${gazebo_dev_LIBRARY_DIRS}
)

# Libraries
add_library(fetch_gazebo_plugin SHARED
  src/plugin.cpp
)
ament_target_dependencies(fetch_gazebo_plugin
  angles
  control_toolbox
  gazebo_ros
  robot_controllers
  robot_controllers_interface
  rclcpp
)

## Install
install(
  TARGETS fetch_gazebo_plugin
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

ament_export_include_directories(
  include
)

ament_export_dependencies(ament_cmake)
ament_export_dependencies(rclcpp)

install(
  DIRECTORY include/${PROJECT_NAME}/
  DESTINATION include/${PROJECT_NAME}
)

install(
  PROGRAMS
    "scripts/prepare_simulated_robot.py"
    "scripts/prepare_simulated_robot_pick_place.py"
  DESTINATION bin
)

install(
  DIRECTORY config launch robots worlds models
  DESTINATION share/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
