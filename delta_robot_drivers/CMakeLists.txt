cmake_minimum_required(VERSION 2.8.3)
project(delta_robot_drivers)

#set C++11 support
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

## Find catkin macros and libraries
find_package( catkin REQUIRED COMPONENTS
      roscpp
      rospy
      image_transport
      cv_bridge
      delta_robot_kinematics)

## System dependencies are found with CMake's conventions
find_package(
  delta_robot_kinematics REQUIRED)

#source files
SET(SRCS
      src/delta_angles_streamer.cpp
      src/delta_angles_streamer_node.cpp)

## The catkin_package macro generates cmake config files for your package
catkin_package(
      CATKIN_DEPENDS
            roscpp
            rospy
            cv_bridge
            delta_robot_kinematics)
            
add_definitions(-DROS=1)  #build using ROS libraries

## Specify additional locations of header files
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)
include_directories(${OpenCV_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})

## Declare a cpp executable for pipol_tracker_node
add_executable(delta_angles_streamer_node
  ${SRCS})
#add_dependencies(pipol_tracker_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${PROJECT_NAME}_generate_messages_cpp)
target_link_libraries(delta_angles_streamer_node ${catkin_LIBRARIES})

#install binaries
install(TARGETS delta_angles_streamer_node
    DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

#Share launch and configs
foreach(dir launch config)
    install(DIRECTORY ${dir}/
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/${dir})
endforeach(dir)
