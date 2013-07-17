cmake_minimum_required(VERSION 2.4.6)
project(turtlebot_calibration)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

find_package(catkin REQUIRED COMPONENTS rospy sensor_msgs nav_msgs geometry_msgs turtlebot_bringup)

#rosbuild_init()

catkin_python_setup()

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()
add_message_files(
  FILES ScanAngle.msg
)

generate_messages(
  DEPENDENCIES std_msgs
)
#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})

catkin_package(
  CATKIN_DEPENDS rospy sensor_msgs nav_msgs geometry_msgs python_orocos_kdl turtlebot_bringup
)

install(PROGRAMS src/turtlebot_calibration/calibrate.py
                 src/turtlebot_calibration/scan_to_angle.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
