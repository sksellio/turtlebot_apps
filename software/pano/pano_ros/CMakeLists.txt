cmake_minimum_required(VERSION 2.4.6)
#include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

project(pano_ros)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE Debug)

#rosbuild_find_ros_package(actionlib_msgs)
#rosbuild_find_ros_package(sensor_msgs)
#include(${actionlib_msgs_PACKAGE_PATH}/cmake/actionbuild.cmake)
#genaction()

#rosbuild_init()

find_package(catkin REQUIRED COMPONENTS std_msgs sensor_msgs actionlib_msgs message_generation)

catkin_python_setup()


#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()

add_action_files(
  FILES Stitch.action
        PanoCapture.action
)

add_message_files(
  FILES Pano.msg
)

generate_messages(
    DEPENDENCIES
        actionlib_msgs
        std_msgs
        sensor_msgs
)



#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()

#find_package(OpenCV REQUIRED)

#rosbuild_add_executable(example examples/example.cpp)
#link_libraries(example ${PROJECT_NAME})
catkin_package(
    CATKIN_DEPENDS std_msgs sensor_msgs actionlib_msgs message_runtime
)

install(PROGRAMS nodes/capture_client.py
                 nodes/capture_server.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

