cmake_minimum_required(VERSION 2.8)
project(object_recognition_msgs)

# generate the messages and the actionlib server for ROS
find_package(catkin REQUIRED)
if (${catkin_VERSION} VERSION_GREATER "0.5.28")
find_package(catkin REQUIRED actionlib_msgs genmsg geometry_msgs rosbag roscpp sensor_msgs shape_msgs visualization_msgs)
else()
        find_package(ROS REQUIRED actionlib_msgs genmsg geometry_msgs rosbag roscpp sensor_msgs shape_msgs visualization_msgs)
        endif()

add_action_files(DIRECTORY action FILES ObjectRecognition.action)
add_service_files(DIRECTORY srv FILES GetObjectInformation.srv)

add_message_files(DIRECTORY msg
                  FILES ObjectId.msg
                        ObjectInformation.msg
                        RecognizedObject.msg
                        RecognizedObjectArray.msg
                        Table.msg
                        TableArray.msg
)

generate_messages(DEPENDENCIES actionlib_msgs geometry_msgs sensor_msgs shape_msgs std_msgs visualization_msgs)

# define the project
find_package(catkin REQUIRED)
if (${catkin_VERSION} VERSION_GREATER "0.5.28")
catkin_package(INCLUDE_DIRS include)
else()
catkin_stack()
catkin_project(object_recognition_msgs
               INCLUDE_DIRS include)
set(catkin_LIBRARIES ${ROS_LIBRARIES})
set(catkin_INCLUDE_DIRS ${ROS_INCLUDE_DIRS})
set(CATKIN_DEVEL_PREFIX ${CMAKE_BINARY_DIR})
set(CATKIN_PACKAGE_BIN_DESTINATION share/${PROJECT_NAME}/bin)
set(CATKIN_PACKAGE_INCLUDE_DESTINATION include/${PROJECT_NAME})
set(CATKIN_PACKAGE_LIB_DESTINATION lib)
set(CATKIN_PACKAGE_PYTHON_BUILD_DESTINATION gen/py/${PROJECT_NAME}/)
set(CATKIN_PACKAGE_PYTHON_DESTINATION ${INSTALLED_PYTHONPATH}/${PROJECT_NAME}/)
set(CATKIN_PACKAGE_SHARE_DESTINATION share/${PROJECT_NAME}/)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
endif()

add_subdirectory(python)

# install the include folder
install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/
        DESTINATION include
        COMPONENT main
)
if (ROS_FUERTE_FOUND)
install(FILES ${PROJECT_SOURCE_DIR}/manifest.xml
        DESTINATION share/${PROJECT_NAME}
)
endif()
