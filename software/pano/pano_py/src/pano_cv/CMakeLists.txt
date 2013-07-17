find_package(Boost REQUIRED COMPONENTS python )
include_directories(${Boost_INCLUDE_DIRS}) 

#include_directories(../../../pano_core ../pano_ros ../../../pano_py)

file(GLOB pano_cv_srcs opencv.cpp mat.cpp numpy_to_mat.cpp)
add_library(pano_cv ${pano_cv_srcs})

target_link_libraries(pano_cv ${Boost_LIBRARIES})

#do this for python module importing
set_target_properties(pano_cv
  PROPERTIES
  PREFIX "")

#rosbuild_link_boost(pano_cv python)
