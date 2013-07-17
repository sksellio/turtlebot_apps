cmake_minimum_required(VERSION 2.4.6)
#include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
project(turtlebot_panorama)

#rosbuild_init()

# Load catkin and all dependencies required for this package
find_package(catkin REQUIRED COMPONENTS roscpp actionlib std_msgs std_srvs sensor_msgs geometry_msgs message_generation image_transport pano_ros)
find_package(Boost REQUIRED COMPONENTS system)
find_package(Eigen REQUIRED)

include_directories(include ${catkin_INCLUDE_DIRS} ${Boost_INCLUDE_DIR} ${Eigen_INCLUDE_DIR})

catkin_python_setup()

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# common commands for building c++ executables and libraries
add_executable(panorama src/panorama.cpp)
add_dependencies(panorama pano_ros_gencpp)
target_link_libraries(panorama ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${Eigen_LIBRARIES})

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()

# services
add_service_files(
  FILES TakePano.srv
)

# Generate added messages and services with any dependencies listed here
generate_messages(
  DEPENDENCIES std_msgs
)

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)

#rosbuild_add_executable(panorama src/main.cpp
#                                 src/pano_app.cpp)
#rosbuild_add_executable(panorama src/panorama.cpp)
#rosbuild_add_executable(panorama_stop src/main2.cpp
#                                 src/pano_app3.cpp)

catkin_package(
    DEPENDS Boost Eigen
    CATKIN_DEPENDS roscpp actionlib std_msgs std_srvs sensor_msgs geometry_msgs message_runtime image_transport pano_ros
    INCLUDE_DIRS include
)

#rosbuild_link_boost(panorama thread)
#rosbuild_link_boost(panorama thread)
#rosbuild_link_boost(panorama_stop thread)

#target_link_libraries(example ${PROJECT_NAME})

install(TARGETS panorama
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(PROGRAMS scripts/loop.py
        DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY launch
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

install(DIRECTORY param
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

