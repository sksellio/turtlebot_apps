find_package(Boost REQUIRED COMPONENTS python)
include_directories(${Boost_INCLUDE_DIRS}) 

#this is the pano_py library/python module
file(GLOB pano_py_srcs *.cpp)
add_library(${PROJECT_NAME} ${pano_py_srcs})

target_link_libraries(${PROJECT_NAME} pano_cv)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES})

#rosbuild_link_boost(${PROJECT_NAME} python)

set_target_properties(${PROJECT_NAME}
  PROPERTIES
  PREFIX "")
