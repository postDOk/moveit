set(MOVEIT_LIB_NAME moveit_constraint_sampler_manager_loader)

add_library(${MOVEIT_LIB_NAME} src/constraint_sampler_manager_loader.cpp)
target_link_libraries(${MOVEIT_LIB_NAME} ${catkin_LIBRARIES})

install(TARGETS ${MOVEIT_LIB_NAME} LIBRARY DESTINATION lib)
install(DIRECTORY include/ DESTINATION include)
