cmake_minimum_required(VERSION 2.8.3)
project(ethzasl_point_cloud_vtk_tools)
find_package(libpointmatcher REQUIRED HINTS /usr/local/share/libpointmatcher)

find_package(catkin REQUIRED 
roscpp
rospy
rosbag
std_msgs
geometry_msgs
sensor_msgs
nav_msgs
tf
tf_conversions
libpointmatcher_ros)

catkin_package(
CATKIN_DEPENDS roscpp rospy rosbag std_msgs geometry_msgs sensor_msgs nav_msgs
tf tf_conversions libpointmatcher libpointmatcher_ros
)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
set(CMAKE_BUILD_TYPE RelWithDebInfo)

# c++ 11
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
        message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support, or our tests failed to detect it correctly. Please use a different C++ compiler or report this problem to the developers. You need partial C++0x support for N2351, N2672, N1984.")
endif()
# well, this breaks libpointmatcher on non-unix platform,
# please contribute the define for your favorite compiler
# It must support (compiler support in parenthesis):
# - N2351: shared pointers in standard
# - N2672: Initializer lists (gcc)
# - N1984: auto-typed variables (gcc, clang)


set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

include_directories(${catkin_INCLUDE_DIRS})

add_executable(pointCloudToVtk src/pointCloudToVtk.cpp)
target_link_libraries(pointCloudToVtk ${libpointmatcher_LIBRARIES} ${catkin_LIBRARIES})

add_executable(vtkToPointCloud src/vtkToPointCloud.cpp)
target_link_libraries(vtkToPointCloud ${libpointmatcher_LIBRARIES} ${catkin_LIBRARIES})


