cmake_minimum_required(VERSION 2.8.3)
project(force_torque_sensor_calib)
add_definitions(-std=c++11)
find_package(catkin REQUIRED COMPONENTS cmake_modules roscpp sensor_msgs tf geometry_msgs moveit_core moveit_ros_planning moveit_ros_planning_interface tf_conversions eigen_conversions)

set (Boost_USE_STATIC_LIBS OFF) 
set (Boost_MULTITHREADED ON) 
set (Boost_USE_STATIC_RUNTIME OFF)
# manually set lib to find thread
set (BOOST_LIBRARYDIR "/usr/lib/x86_64-linux-gnu")
find_package(cmake_modules REQUIRED)
find_package(Eigen REQUIRED)
find_package(Boost 1.58 REQUIRED COMPONENTS system filesystem date_time thread)

catkin_package(
  DEPENDS Eigen Boost
  CATKIN_DEPENDS roscpp sensor_msgs tf geometry_msgs moveit_core moveit_ros_planning moveit_ros_planning_interface eigen tf_conversions eigen_conversions
  INCLUDE_DIRS include
  LIBRARIES
)


include_directories(SYSTEM ${Boost_INCLUDE_DIR})
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
)

file(GLOB_RECURSE HEADERS include/${PROJECT_NAME}/*.h)

add_executable(ft_calib_node src/ft_calib.cpp src/ft_calib_node.cpp ${HEADERS})
target_link_libraries(ft_calib_node ${Boost_LIBRARIES} ${catkin_LIBRARIES})

install(TARGETS ft_calib_node
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY config include launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
