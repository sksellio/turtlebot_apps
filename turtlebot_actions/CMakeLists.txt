cmake_minimum_required(VERSION 2.4.6)
project(turtlebot_actions)

#list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
#find_package(Eigen REQUIRED)
#include_directories(${EIGEN_INCLUDE_DIRS})

#include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

#rosbuild_find_ros_package(actionlib_msgs)
#include(${actionlib_msgs_PACKAGE_PATH}/cmake/actionbuild.cmake)
#genaction()

find_package(catkin REQUIRED COMPONENTS roscpp actionlib actionlib_msgs geometry_msgs message_generation tf cv_bridge image_transport image_geometry turtlebot_bringup)
find_package(Boost REQUIRED COMPONENTS system)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
find_package(Eigen REQUIRED)
include_directories(include ${Boost_INCLUDE_DIR} ${catkin_INCLUDE_DIRS} ${EIGEN_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS})

catkin_python_setup()


#rosbuild_init()

#rosbuild_add_boost_directories()

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

add_action_files(
    DIRECTORY action
    FILES FindFiducial.action
          TurtlebotMove.action
)

generate_messages(
    DEPENDENCIES
        actionlib_msgs
        std_msgs
        geometry_msgs
)


#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()

add_executable(turtlebot_move_action_server src/turtlebot_move_action_server.cpp)
target_link_libraries(turtlebot_move_action_server ${Boost_LIBRARIES} ${catkin_LIBRARIES})
add_dependencies(turtlebot_move_action_server turtlebot_actions_gencpp)

add_executable(find_fiducial_pose src/find_fiducial_pose.cpp src/detect_calibration_pattern.cpp)
target_link_libraries(find_fiducial_pose ${Boost_LIBRARIES} ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
add_dependencies(find_fiducial_pose turtlebot_actions_gencpp)

catkin_package(
    DEPENDS eigen opencv
    CATKIN_DEPENDS roscpp actionlib actionlib_msgs geometry_msgs message_runtime tf cv_bridge image_transport image_geometry turtlebot_bringup
    INCLUDE_DIRS include
)




#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(turtlebot_move_action_server src/turtlebot_move_action_server.cpp)
#rosbuild_link_boost(turtlebot_move_action_server thread)
#rosbuild_add_executable(turtlebot_goto_action_server src/turtlebot_goto_action_server.cpp)

#rosbuild_add_executable(find_fiducial_pose src/find_fiducial_pose.cpp src/detect_calibration_pattern.cpp)
#rosbuild_link_boost(find_fiducial_pose thread)
#find_package(OpenCV)
#target_link_libraries(find_fiducial_pose ${OpenCV_LIBRARIES})

#rosbuild_add_executable(follow_fiducial test/follow_fiducial.cpp)
#target_link_libraries(example ${PROJECT_NAME})

install(TARGETS turtlebot_move_action_server find_fiducial_pose 
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

install(PROGRAMS scripts/test_fiducial.py
                 scripts/test_move.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

