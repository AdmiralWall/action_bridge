cmake_minimum_required(VERSION 3.5)
project(action_bridge)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_action REQUIRED)

find_package(example_interfaces REQUIRED)
find_package(control_msgs REQUIRED)

# find ROS 1 packages
include(cmake/find_ros1_package.cmake)

find_package(PkgConfig REQUIRED)

find_ros1_package(roscpp REQUIRED)
find_ros1_package(actionlib REQUIRED)

find_ros1_package(actionlib_tutorials REQUIRED)
find_ros1_package(control_msgs REQUIRED)


ament_export_include_directories(include)

include_directories(include)

add_executable(action_bridge_follow_joint_trajectory_node
    "src/action_bridge_follow_joint_trajectory.cpp"
)
ament_target_dependencies(action_bridge_follow_joint_trajectory_node
    "ros1_roscpp" "ros1_actionlib" "ros1_control_msgs"
    "rclcpp" "rclcpp_action" "control_msgs"
)

add_executable(action_bridge_fibonacci_node
    "src/action_bridge_fibonacci.cpp"
)
ament_target_dependencies(action_bridge_fibonacci_node
    "ros1_roscpp" "ros1_actionlib" "ros1_actionlib_tutorials"
    "rclcpp" "rclcpp_action" "example_interfaces"
)

install(TARGETS action_bridge_follow_joint_trajectory_node action_bridge_fibonacci_node
    DESTINATION lib/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # remove the line when a copyright and license is present in all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # remove the line when this package is a git repo
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
