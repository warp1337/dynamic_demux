cmake_minimum_required(VERSION 2.8)
project(dynamic_demux)

## Find system dependencies
find_package(cmake_modules REQUIRED)
find_package(Boost REQUIRED COMPONENTS system filesystem thread)

## Find catkin packages
find_package(catkin REQUIRED COMPONENTS
  dynamic_reconfigure
  genmsg
  nodelet
  nodelet_topic_tools
  pcl_conversions
  pcl_msgs
  pluginlib
  rosconsole
  roscpp
  roslib
  sensor_msgs
  std_msgs
)

## Add include directories
include_directories(include
  ${Boost_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
)

generate_dynamic_reconfigure_options(
    cfg/NodeletDynamicDemux.cfg
)

## Declare the catkin package
catkin_package(
  INCLUDE_DIRS include
  LIBRARIES
    did
  CATKIN_DEPENDS
    dynamic_reconfigure
    message_filters
    nodelet
    roscpp
    sensor_msgs
    std_msgs
  DEPENDS
    Boost
)

## Nodelet

add_library(dynamic_demux
  src/dynamic_demux.cpp
)
target_link_libraries(dynamic_demux ${catkin_LIBRARIES} ${Boost_LIBRARIES})
class_loader_hide_library_symbols(dynamic_demux)

install(
  TARGETS
    dynamic_demux
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)

install(DIRECTORY include/nodelet_dynamic_demux/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

install(FILES dynamic_demux_nodelet.xml DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

install(DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
  PATTERN ".svn" EXCLUDE)