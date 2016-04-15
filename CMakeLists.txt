cmake_minimum_required(VERSION 2.8.3)
project(ximea_camera_opencv_example)

FIND_PACKAGE(catkin REQUIRED)
FIND_PACKAGE(roscpp REQUIRED)
FIND_PACKAGE(cv_bridge REQUIRED)
FIND_PACKAGE(image_transport REQUIRED)
FIND_PACKAGE(OpenCV 2.4 REQUIRED)
#MESSAGE("OpenCV version (${OpenCV_VERSION})")

IF(NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/package.xml")
    MESSAGE(FATAL_ERROR "${CMAKE_CURRENT_SOURCE_DIR}/package.xml not found.")
ENDIF()


set(SOURCE_FILES src/view_ximea_camera.cpp)
add_executable(${PROJECT_NAME} ${SOURCE_FILES})
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES}
                                      ${roscpp_LIBRARIES}
                                      ${cv_bridge_LIBRARIES}
                                      ${image_transport_LIBRARIES})