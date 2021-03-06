cmake_minimum_required(VERSION 2.8.3)
project(chomp_interface)

add_definitions(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  cmake_modules
  roscpp
  moveit_core
  pluginlib
  chomp_motion_planner
  moveit_experimental
)

find_package(Eigen3 REQUIRED)

# Eigen 3.2 (Wily) only provides EIGEN3_INCLUDE_DIR, not EIGEN3_INCLUDE_DIRS
if(NOT EIGEN3_INCLUDE_DIRS)
  set(EIGEN3_INCLUDE_DIRS ${EIGEN3_INCLUDE_DIR})
endif()

find_package(Boost REQUIRED)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES chomp_interface
  CATKIN_DEPENDS roscpp moveit_core pluginlib
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS} ${EIGEN3_INCLUDE_DIRS}
)

add_library(chomp_interface src/chomp_interface.cpp src/chomp_planning_context.cpp)
target_link_libraries(chomp_interface ${catkin_LIBRARIES})

add_library(chomp_planner_plugin src/chomp_plugin.cpp)
target_link_libraries(chomp_planner_plugin chomp_interface ${catkin_LIBRARIES})

install(FILES chomp_interface_plugin_description.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

install(TARGETS chomp_interface chomp_planner_plugin
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
